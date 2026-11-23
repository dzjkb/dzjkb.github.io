2018 - Defossez (Facebook AI)
[paper](https://arxiv.org/pdf/1810.09785)
conceptually the same thing as [[NSynth]]?
- decoder generating single notes from instrument + pitch + velocity conditioning signal
- the difference would be the instrument signal? NSynth does a single decoder per instrument
- introduces spectral loss - not present in NSynth, improved upon in [[DDSP]]? (multiscale)
- parallel (not auto-regressive like NSynth), faster
generates at a 16kHz sampling rate

what's interesting - the convolutional decoder (waveform generator) isn't wavenet style
which makes sense if you think about it - it's generated based on an lstm-generated sequence, not autoregressive - no need for the massive receptive field

LSTM sequence is this weird shit
## evaluation
### experiments
- reconstruction quality vs WaveNet
    - MOS
- similarity - ABX similarity measure
- pitch generalization