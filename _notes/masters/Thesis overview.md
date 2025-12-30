The general idea is to create an audio VAE working in the waveform domain (inspired by [[RAVE]] and [[SoundStream]] architectures) compressing short, fixed-length samples (1-3 seconds) into 1-dimensional vectors of length N. Instead of working on streamed music, this model works on samples in the music production sense - short clips of individual instruments or sound effects which form the building blocks of most modern music. By learning a hidden representation of these samples regularized to follow a probability distribution, the model is able to generate novel samples inspired by the training set (similar to [[DrumGAN]]). Additionally, the hidden representation can be manually manipulated with near immediate auditory feedback, enabling manual search of the generation space, effectively turning the model into an instrument. The work will explore using normalizing flows to increase the expressiveness, quality and diversity of the model.

Idea summary:
- start from [[RAVE]]
- focus on new datasets of short, fixed-length samples instead of continuous audio streams
- use normalizing flows for a more flexible posterior
- use normalizing flows for a more flexible prior, training it in the _second_ training stage once the encoder (=> posterior) is frozen
- evaluate unconditional generation using MOS, KID, FAD and possibly other metrics that measure either quality and _diversity_
    - using [CLAP](https://github.com/LAION-AI/CLAP) embeddings for FAD/KID
    - compare generation with/without NF priors, posteriors
- write about the whole thing from the perspective of creative audio AI in music

The work will focus on:
- evaluating the quality and diversity of unconditionally generated samples
- evaluating the performance of residual-dilated convolutional architectures working in the waveform domain on short audio samples
- evaluating the performance gain normalizing flows can provide for richer prior/posterior estimation
- exploring methods to disentangle the hidden representation and adapt the model for use as an instrument

continuing in [[2025-12-30-masters-overview]]