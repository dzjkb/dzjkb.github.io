these guys fr http://acids.ircam.fr/

main repo - https://github.com/acids-ircam/RAVE/tree/master
main paper - https://arxiv.org/abs/2111.05011

closely related - cached convolutions
repo - https://github.com/acids-ircam/cached_conv/
paper - https://arxiv.org/pdf/2204.07064

with their own [pytorch version of ddsp](https://github.com/acids-ircam/ddsp_pytorch) hhnnnggg
## summary
**architecture**
- traditional VAE - mean + variance encoding
- using noise synth from ddsp + wave and loudness subnets
- wave subnet is just a fucking convolution? bro I swear
- filtered noise is the only ddsp component
- signal is split into **TODO** bands which brings the massive speedup
    - PQMF filters (**TODO** link)
    - each band is downsampled, hence the speedup

**latent space**
- sampled at 20Hz using a SampleRNN

**two stage training**
- first everything is trained on spectral distance loss, similar to DDSP
- then, the encoder is frozen and the whole thing is fine-tuned using an additional _discriminator network_

**other details**
- `ratio` stands for factors to multiply initial channel count by
- `n_out = 2` is for outputing 2 latent vectors - mean and logvar
- looks like `n_channels == 1`? it's not set in any gin config
- and `N_BANDS = 16` which are just represented as different channels for convolutions, which gives `data_size = 16`, so the input shape is `(L, data_size)`?

## cached convolutions
adapting fixed input length models to stream processing without retraining
- basically copying buffers over as left padding
- and swapping right-padding for left-padding
    - with an additional delay (lag) if stride > 1 to preserve layer behavior
- delay cumulated throughout the whole model used for delaying the output later
## code
main model class - `model.RAVE` - a lightning module
- using `pl.Trainer` for training, most of the train script is reading configs and creating callbacks
- configured through gin passing in `__init__()` arguments
encoder - `blocks.VariationalEncoder`
- which uses the `Encoder` or `EncoderV2`
decoder - `blocks.Generator` or `blocks.GeneratorV2`
- using the `NoiseGeneratorV2`, which doesn't seem to be using ddsp??
- "as proposed by Engel et al", huh might be the case they just took the mechanism/idea?
- tough math

## encoder
based on perhaps [SING](https://arxiv.org/pdf/1810.09785)?
all `Conv1D`s have weight norm applied

```
kernel_size = 3
capacity (start channels) = 96
latent_size = 128

    [ Conv1D ] out_c = capacity, k = kernel_size*2+1, padding = (k,k)
        |      L_out = L_in
        v
[ Encoder block ] x 4 w/ ratios (4, 4, 4, 2) (capacity *2, *4, *8, *16 channels
        |         which gives 1536 channels at the end
        v
   [ LeakyReLU ]
        v
    [ Conv1D ] out_c = latent_size*2, k = kernel_size, padding = ((k-1)//2,k//2)
               L_out = L_in

Encoder block w/ in_c, r:
    [ Residual dilated unit ] x 3 for the first 3 layers, 2 for the fourth one
            |                 with (1, 3, 9) dilations
            v
       [ LeakyReLU ]
            v
        [ Conv1D ] out_c = in_c * 2, k = 2*r, stride = r, padding = (r-1, r)
                   L_out = floor((L_in - 3) / r + 1)

Residual dilated unit w/ dilation d - preserves dimensions w/ proper padding:
    --> [ LeakyReLU ] -> [ Dilated Conv1D ] -> [ LeakyReLU ] -> [ Conv1D ] -> +
     |                  k=kernel_size, d=d                         k=1        ^
      \ ----------------------------------------------------------------------+
```

## decoder
based on [MelGAN](https://arxiv.org/pdf/1910.06711)
all `Conv1D`s have weight norm applied

```
kernel_size = 3

    [ Conv1D ] out_c = capacity*2^4, k = kernel_size, padding=(k-1//2, k//2)
        |      L_out = L_in
        V
[ Decoder block ] x 4 w/ ratios (4, 4, 4, 2) (capacity /2, /4, /8, /16)
        v
   [ LeakyReLU ]
        v -------------------v
[ Waveform module ]  [ Noise generator ]
         \-------> + <------/
               [ tanh ]

Decoder block w/ in_c, r:
       [ LeakyReLU ]
            v
    [ Conv1D transposed ] out_c = in_c//2, k = 2*r, stride = r, padding = r//2
            |             L_out = L_in * r
            v
  [ Residual dilated unit ] x 3 for the first 3 layers, 2 for the fourth one
                            with (1, 3, 9) dilations

Waveform module (with optional amplitude modulation):
    [ Conv1D ] out_c = 2, k = kernel_size*2+1, padding = (k, k), L_out = L_in
         \--> (x, amplitude) -> x * sigmoid(amplitude)

Noise generator:
    [ Conv1D + LeakyReLU ] x 2
            v
        [ Conv1D ]
            v  (filter amps)
  [ amps -> filtered noise ]
```
## downsampling factor of the encoder
```
padding = (r - 1, r)
L_out = floor((L_in + 2r - 1 - 2r - 2) / r + 1)

r = 4 => L_out = floor((L_in - 3) / 4 + 1) = floor((L_in + 1) / 4)
r = 2 => L_out = floor((L_in - 3) / 2 + 1) = floor((L_in - 1) / 2)
```

which after 4 blocks with (4, 4, 4, 2) ratios gives
```
L_in = 3000 (x 2 channels) (after 16-band decomposition)
L_out = 23 (x 128 latent dimensions x 2 "outputs" (mean, var))
```

~~hm the paper says its 23Hz not 344Hz?~~
- [x] what's the actual latent space rate?
## upsampling factor of the decoder
```
padding = r//2
L_out = (L_in - 1)*r - r + 2r = L_in*r
```

which after 4 blocks with (4, 4, 4, 2) ratios gives
```
L_in = 23 (x 128 latent dimensions)
L_out = 23*128 = 2944
```
- [ ] why's the decoded rate 2944Hz not 3kHz - seems that the loss code expects the input/output to have the same rate (maybe `_pqmf_decode()` does some adjusting?)
## dataset
for dataset purposes RAVE uses:
- `lmdb` - holding basically a name -> waveform mapping?
    - with metadata probably
- `udls` - an internal library providing an `AudioExample` class
    - this is what's stored in the db
    - contains the waveform, metadata etc.