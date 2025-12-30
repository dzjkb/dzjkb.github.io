2020 - continuation of [[NSynth]]
https://github.com/magenta/ddsp
still trained on single notes
### filtered noise
```
"""Synthesize audio with filtered white noise.

Args:
    magnitudes: Magnitudes tensor of shape [batch, n_frames, n_filter_banks].
        Expects float32 that is strictly positive.

Returns:
    signal: A tensor of harmonic waves of shape [batch, n_samples, 1].
"""
```
defined [here](https://github.com/magenta/ddsp/blob/main/ddsp/synths.py#L150)
high level steps overview:
```
x (magnitudes) -> scale -> IR -> window the IR -> convolve noise with IR
```
impulse response defined [here](https://github.com/magenta/ddsp/blob/main/ddsp/core.py#L1534)
- "follows approach in https://ccrma.stanford.edu/~jos/sasp/Windowing_Desired_Impulse_Response.html"
- 65 channels as input magnitudes
- also -5 bias, ok
what are the magnitudes
- shape `(batch_size, n_frames, n_frequencies)`
- frequencies are split into equal intervals between 0 and nyquist
- `fft_convolve` [here](https://github.com/magenta/ddsp/blob/main/ddsp/core.py#L1382) describes how the time-varying filtering works
- ![[Pasted image 20250727144412.png]]
### losses
only L1 differences between spectrograms and log spectrograms
# pytorch DDSP from ICRAM
note: the RAVE filtered noise synth also has to operate on the multiple bands (after `pqmf`)
### filtered noise
what is the noise generator anyway
```
x
    -> conv1d stack
    -> 2 * sigmoid(x - 5)**2.3 + 1e-7
    -> 
```

kind of doesn't look like it's doing the same frame-splitting as in the original DDSP?
does the `torch.fft.irfft()` actually just compute an IR per each timestep? instead of explicitly specifying `n_frames` like in the original DDSP
wait no fuck, the conv stack downsamples the signal, effectively making `n_frames = prod(ratios)`, nice

ok why is `noise` of the same shape as the `ir` - shouldn't it have more timesteps?
is this just effectively instantiating the noise as split into frames already, convolving each one separately and then concatenating them by reshaping at the end???
jesus christ Antoine why do u do this to me
- clever cunt
# goals/evaluation
mainly reconstruction quality?
- [x] what are the goals and evaluation methods of DDSP?

**achieved goals**
- higher reconstruction quality
    - on the NSynth dataset
    - using novel methods - domain knowledge/inductive bias
    - without adversarial loss, with smaller models
- disentaglement
    - loudness/pitch
    - "alters samples along a matching perceptual axis" taking that fr
side "achievements"
- deconstructing incoming audio into components (e.g. dereverberation)
- timbre transfer

**metrics**
- loudness vector and pitch vector L1 distance
    - they don't actually quantitatively compare the timbre in evalution
    - pitch only in places where there was detectable pitch content, suggested by F0 confidence > 0.85 from CREPE
- F0 outliers
    - i.e. the number of regions that couldn't be pitch-detected, complementary to the pitch distance above
- parameter count
- interpolation metrics - loudness/pitch distances with substituted loudness/pitch/Z vectors
    - measuring disentaglement mostly?
    - i.e. the loudness reconstruction will still be good if we supply a different external Z or pitch

comparing with only [[WaveRNN]], hm