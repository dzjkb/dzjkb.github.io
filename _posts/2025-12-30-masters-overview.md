---
title: Master's thesis overview
date: 2025-12-30
feed: show
category: posts
layout: Post
---
The general idea is to create an audio VAE working in the waveform domain (inspired by [RAVE](https://github.com/acids-ircam/RAVE/tree/master) and [SoundStream](https://github.com/acids-ircam/RAVE/tree/master) architectures) compressing short, fixed-length samples (1-3 seconds) into a latent space of dimensionality N. Instead of working on streamed music, this model works on [samples](https://en.wikipedia.org/wiki/Sampling_(music)) in the music production sense - short clips of individual instruments or sound effects which form the building blocks of most modern music. Contrary to RAVE/SoundStream, the whole sample is compressed into a single latent vector instead of a time series, which will make the latent space capture different - global - properties of the samples and enable typical VAE unconditional generation by sampling from the prior, instead of requiring something akin to the auto-regressive prior learned by RAVE.

Before passing a latent vector sampled from the prior through the decoder, its values can be manually modified, in turn modifying the generated sample based on the learned latent space structure. We can imagine a setting where an interface enables the manipulation of different dimensions with immediate auditory feedback, effectively turning the model into an instrument (similar to a [sampler](https://en.wikipedia.org/wiki/Sampler_(musical_instrument))). This aspect makes the work conceptually similar to [DrumGAN](https://github.com/acids-ircam/RAVE/tree/master), which explored generating percussive sounds and capturing meaningful properties in the latent space.

The work will explore using normalizing flows to increase the quality and diversity of the generated samples.

---
# Idea summary
- start from a [RAVE](https://github.com/acids-ircam/RAVE/tree/master)-like architecture
- focus on new datasets of short, fixed-length samples instead of continuous audio streams
- use a single vector latent space instead of a time series
- use normalizing flows (e.g. [Real NVP](https://arxiv.org/abs/1605.08803)) for a more flexible/expressive posterior
- use normalizing flows for a more flexible/expressive prior, training it in the _second_ training stage once the encoder (=> posterior) is frozen
    - similar to what [Learning Energy-based Variational Latent Prior for VAEs](https://arxiv.org/html/2510.00260) does, but simpler
- evaluate unconditional generation using MOS, KID, FAD and possibly other metrics that measure either quality or _diversity_
    - using [CLAP](https://github.com/LAION-AI/CLAP) embeddings for FAD/KID
    - compare generation capabilities with/without NF priors, posteriors
- write about the whole thing from the perspective of creative audio AI in music

\
Main contributions:
- evaluating SOTA audio models on short samples, with a music production focus
- focusing on unconditional generation, not reconstruction quality
- combining SOTA audio models with normalizing flows

# Datasets
Datasets will be possibly created from:
- free sample packs, available online, which contain sounds from a specific category - percussive, synth leads, chimes, etc.
- audio effects from the [LAION-Audio-630K](https://github.com/LAION-AI/audio-dataset) dataset
- custom synth recordings produced by me
- ...?
