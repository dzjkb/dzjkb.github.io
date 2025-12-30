2019 - Vasquez
[paper](https://arxiv.org/pdf/1906.01083)
generative model in the frequency domain - mel spectrograms
something like PixelCNN + RNNs later, idk
speech, piano music (MAESTRO dataset)

https://audio-samples.github.io/
- fuck sounds good actually what
## misc
(!) compares VAEs (as an element of the pipeline) with a **global z** vs a sequence of **local z**'s
refers to [this paper (2018)](https://arxiv.org/pdf/1810.07217), which does VAEs for TTS with 2 levels of latent variables - aiming for disentaglement, with the second level using GMMs
- strictly TTS though probably?
## evaluation
### experiments
- unconditional speech/piano generation
    - qualitative eval only, hm
- density estimation - negative log-likelihood
- preference voting for "structure"