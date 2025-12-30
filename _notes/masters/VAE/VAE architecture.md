code is the best documentation innit
# References
references: VAE with flows, RAVE, SoundStream
- [x] find more references for encoder/decoder architecture choices
# Architecture
choices
- same arch - residual dilated convs - as RAVE/SoundStream
- same downscaling ratios as SoundStream
- 128 latent size as in RAVE
- no MFCCs because non-harmonic audio
- ~~no PQMF for now~~
- simple `/stride` and `*stride` down/upsampling
- non-causal (i.e. classical) convolutions
- short fixed-length audio
- snake activation function?
    - first in [BigVGAN (2022)](https://arxiv.org/abs/2206.04658)
    - used in [[RVQGAN]], and then in v3 [[RAVE]]
    - snake(x) = x + 1/α sin2(αx)
- adain?
    - more in [[2025-11-06]], not sure why but if it helps?
- pqmf
### checkerboard artifacts discussion
mentioned in [[MelGAN]], some discussion in [[WaveGAN]] too, [distill article](https://distill.pub/2016/deconv-checkerboard/) (2016) on the topic, also see [[2025-09-28]], [[2025-10-17]]
summary:
- artifacts arise naturally if stride doesn't divide kernel size
- even if it does, it's easy for the weights to form in a way that produces artifacts, probably milder ones though
- in the context of GAN training, this might produce phase patterns that are easy for the discriminator to identify
- but they might also produce high-frequency artifacts (=> noise if we're working with audio)
- convolution gradients might suffer from the same artifacts => some neurons can get arbitrarily more signal than other ones
- idk testing resize+conv instead might be too much, RAVE/other modern audio models get good results without it
- what do the papers do?
whole paper on the topic: [Upsampling artifacts in neural audio synthesis (arxiv)](https://arxiv.org/pdf/2010.14356)
### discriminators
based on [[RVQGAN]]
1. **MSD** (multi-scale waveform discriminator)
    1. _note:_ this isn't actually used in [[RAVE]], since the default arguments of `DescriptDiscriminator` for this class are `[]`
2. **MPD** (multi-period waveform discriminator)
3. **MRSD** (multi-resolution spectral discriminator) (`MRD` in [[RAVE]] code)

feature matching - plain old mean difference

**MPD** stacks the waveform into `p = 2, 3, 5, 7, 11` columns
- more precisely speaking - a shape `N` 1-D array into a 2-D `N/p, p` array 
so one column contains samples with interval `p` between them
which seems quite reasonable to recognize periodic patterns, the period seem small but w/e
- 11 samples at 48kHz would be ~4363Hz
- oh ok but stacking the convolutions further gives the smaller frequency multiples (SR / 2, / 4, / 8 etc)
- 5 convs + post conv

**MRSD** does STFT (`[2048, 1024, 512]` frame sizes), splits into 5 bands and runs the same stack of convolutions on each one -> concat and the last conv on the whole thing
### latent space
instead of a 23Hz time series of latents - one latent vector
### normalizing flows
using [[Real NVP]]
described in [[2025-12-24]], [[2025-12-28]]
# Training
choices
- try no discriminator/quality fine-tuning first, hopefully isn't necessary in our case
- ok might be necessary afterall
### augmentations
[[RAVE]] does:
- random cropping
- random phase mangling
    - fuckin math man what is this
    - no seriously any explanation???
    - even the [lfilter docs](https://docs.scipy.org/doc/scipy/reference/generated/scipy.signal.lfilter.html) give me no clue, "IIR or FIR filter (...) works for (...) Object type" oh ok guess I'll just go fuck myself
    - ok thanks [claude u the man](https://claude.ai/chat/920d9be3-f710-40d4-a5f7-a326a6649b2f)
- dequantization (16 bits)
    - wtf
    - it's adding random noise less at more than 16bit resolution
    - typical learning noise I guess? sure
also something implemented but turned off in `v3` is random pitch shift, good augmentation for sure
# Improvements TODO
- [x] weight norm (moved from [[2025-07-06]])
    - as proposed in [[MelGAN]]
- [x] amp mod configurable (moved from [[2025-06-28]])
- [x] variational optional (moved from [[2025-07-03]])
- [x] basic augmentations in `AudioDataset` (moved from [[2025-05-04]])
    - normalizing audio? flips? crops? see RAVE with its own aug classes
- [x] add discriminator losses
- [x] add pqmf decomposition
- [x] add the snake activation function
- [x] add validation loss logging
- [ ] add stereo audio validation logging to the local fs
    - or maybe tensorboard supports logging arbitrary files?
- [x] reduce `z` to single latent vector
- [x] add normalizing flows to both posterior and prior distributions
    - Real NVP for starters