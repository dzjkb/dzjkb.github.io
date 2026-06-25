`valid_signal_crop` in RAVE - what it does and when to disable it
## Context

While training RAVE on the `vctk_words` dataset (1s padded speech clips), training crashed at step 0 inside `multiscale_stft` with a zero-element reshape error. Root cause: `valid_signal_crop` was producing an empty tensor for the multiband loss because the receptive field exceeded the multiband signal length. 

The fix was to set `rave.RAVE.valid_signal_crop = False` in `jpconfig.gin`. This document explains what was disabled and why it's safe to do so for this dataset.

## What `valid_signal_crop` does

Convolutional networks have a **receptive field**: each output sample depends on a window of input samples. For the RAVE v2 config in this project, `rave.core.get_rave_receptive_field` measured the receptive field to be: 

- left: 27,117 samples
- right: 26,428 samples
- total: ~53,545 samples �~I~H 563 ms @ 48 kHz 

That means an output sample at position `t` depends on input samples in roughly `[t - 27117, t + 26428]`.

When the model processes a **finite-length** input (rather than an infinite stream), the very first and last output samples don't have the full receptive field of input available �~@~T they fall off the edge. The convolution layers handle this by **padding** the input (with zeros, reflection, or constants) so the convolution math can still be applied. RAVE specifically uses causal / non-causal padding internally.

The output at those edge positions is therefore computed partly from real signal and partly from padding artifacts. `valid_signal_crop` removes those edge samples before computing the audio reconstruction loss, so only the inner region �~@~T the samples that saw a full receptive field of real input on both sides �~@~T contributes to the gradient. It's the standard "valid convolution" idea (as opposed to "same" or "full" convolution) applied at the loss level.

It's enabled in `rave/configs/v2.gin:83` (`valid_signal_crop = True`) and used in `rave/model.py:321-329`:

```python
if self.valid_signal_crop and self.receptive_field.sum():
    x_multiband = rave.core.valid_signal_crop(x_multiband, *self.receptive_field)
    y_multiband = rave.core.valid_signal_crop(y_multiband, *self.receptive_field)
```

The crop function (`rave/core.py:220-225`) divides the receptive field by the multiband channel count (16) and slices both ends:

```python
def valid_signal_crop(x, left_rf, right_rf):
    dim = x.shape[1]
    x = x[..., left_rf.item() // dim:]
    if right_rf.item():
        x = x[..., :-right_rf.item() // dim]
    return x
```

## Why "invalid" edges are misleading in the loss

Two reasons the edge samples aren't useful training signal:

1. **The autoencoder reconstruction at the edges is artificially worse.** The encoder receives padded input �~F~R encodes a representation that's distorted near the edges �~F~R the decoder reconstructs an output that's distorted near the edges. None of this distortion reflects what would happen on a long, "real" signal where the model sees genuine context on both sides.

2. **Penalizing those distorted edges is training the model to fix the wrong thing.** If the multiband audio loss compares the model's output to the input over the full multiband window, and a large fraction of that window is dominated by padding artifacts, the model spends gradient capacity learning to "correct" something that isn't actually a flaw in the learned representation �~@~T it's just an artifact of how convolutions handle finite inputs. 

`valid_signal_crop` exists to focus the loss on the receptive-field-clean interior region.

## Why it failed for `vctk_words`
The training command used `--n_signal 32768` (the largest power of 2 that fits within 1s padded clips of 48000 samples). After PQMF (`N_BAND = 16`), the multiband signal is `32768 / 16 = 2048` samples. After `valid_signal_crop`:

- left crop: `27117 // 16 = 1694` samples
- right crop: `26428 // 16 = 1651` samples
- multiband length after crop: `2048 - 1694 - 1651 = -1297` (clamped to 0 in practice)

The result is an empty tensor. It's then handed to the multiscale STFT, which calls `waveform.reshape(-1, shape[-1])` with `shape[-1] == 0`, raising:

```
RuntimeError: cannot reshape tensor of 0 elements into shape [-1, 0]
```

For `valid_signal_crop` to leave a non-empty multiband region greater than the largest STFT n_fft (2048), we'd need `n_signal / 16 > 1694 + 1651 + 2048`, i.e. `n_signal > 86288`. That requires clip lengths well beyond 1s, so it's incompatible with the dataset as built.

## What is lost by disabling it

With `valid_signal_crop = False`, the multiband loss is computed over **all 2048 samples** of the multiband signal, edges included. Practical implications:

- The model will see a loss signal computed partly over padding-affected regions. It'll spend some gradient capacity learning to handle the specific padding pattern at the boundaries of 1s clips.
- For a model meant to be deployed on long-form audio, this is a slight train/deployment mismatch: training-time edges differ from deployment-time edges. For a model trained on, and deployed to generate, ~1s clips, it's actually *more* representative of the deployment distribution. 
- Most reference vocoders (HiFi-GAN, EnCodec, RAVE v1) don't have this crop. It's a v2 refinement, not a correctness requirement.

For short-clip speech-synthesis training, the cost is mostly theoretical.

## The change

In `RAVE/rave/configs/jpconfig.gin`:

```gin
rave.RAVE:
    sampling_rate = %SAMPLING_RATE
    valid_signal_crop = False
```