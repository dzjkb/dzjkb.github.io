2020 - Yamamoto
[paper](https://arxiv.org/pdf/1910.11480)
**not** related closely to [[WaveGAN]] actually, bruh
builds on (and compares against) [[ClariNet]]
- removing distillation need
- adversarial training

summary:
- speech generation
- parallel as in not autoregressive
- real-time, 24kHz sampling rate
- multi-scale spectra + adversarial loss
- MOS
- conditioned on mel spectrograms

[github impl](https://github.com/kan-bayashi/ParallelWaveGAN)
## architecture
### decomposition
[pqmf.py](https://github.com/kan-bayashi/ParallelWaveGAN/blob/master/parallel_wavegan/layers/pqmf.py)
## evaluation
- [x] what are the evaluation methods/metrics, compared with what models?
### experiments
- TTS
### metrics
[[Mean Opinion Score]]
inference speed