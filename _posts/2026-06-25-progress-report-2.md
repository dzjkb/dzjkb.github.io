---
title: Thesis progress report 2
date: 2026-08-06
feed: show
category: posts
layout: Post
---
**Current status:**
- finished training models on VCTK words, percussive and sfx datasets
- ran latent space analysis for VCTK words and percussive
- failed to reproduce DrumGAN
- normalizing flow priors provide a massive quality boost for unconditional generation, surpassing RAVE and non-NF models on all datasets so far

<br>
**Latent space analysis findings**
- properties linearly extractable from latent space:

| dataset    | high acc                                       | medium acc          | low acc     |
| ---------- | ---------------------------------------------- | ------------------- | ----------- |
| VCTK words | duration, gender                               | word                | accent      |
| Percussive | instrument family, decay, dynamics, brightness | spectral complexity | harmonicity |
| SFX        | length, dynamics, brightness                   | spectral complexity | harmonicity |
- instrument family for the percussive dataset is even transferable from an external dataset

---
### Latent space analysis

i.e. analysis on validation set samples encoded into the latent space by the trained encoders. Did exploratory analysis of the distributions as well as two experiments:
- extracting features from the latent space via linear classifiers which were either given in metadata or extracted via `essentia`
- _strictly for the Percussive dataset_: training a kick/snare/hat classifier on a different, very clean dataset and running it on the kick/snare/hat subset of the Percussive val set

<br>
a note on features extracted using `essentia`:
- 
<br>
findings:
- normalizing flows posteriors don't help quality much, but smear the information over more of the latent dimensions (more effective latent space utilization?)
- fidelity - as defined in [[RAVE]] - is very high compared to their values

<br>
linear feature extraction results:
#### VCTK words
all features analyzed here are from the dataset metadata itself

| feature  | accuracy |
| -------- | -------- |
| duration |          |
| gender   |          |
| word     |          |
| accent   |          |
<br>
#### Percussive
the instrument family is taken from metadata (not all samples had it), the rest is from `essentia`

| feature           | accuracy |
| ----------------- | -------- |
| instrument family |          |
| decay             |          |
| dynamics          |          |
| brightness        |          |
|                   |          |
|                   |          |
<br>
#### SFX
all features analyzed here are from `essentia`

| feature | accuracy |
| ------- | -------- |
|         |          |
|         |          |
|         |          |
|         |          |
|         |          |


---
### The research hypothesis

- if we take a [[RAVE]]-like VAE and reduce the latent representation to a single vector we can achieve similar reconstruction/uncond. generation quality
- additionally the VAE extracts high-level, global (as in pertaining to the whole clip duration) features, which can be then used to control the synthesis process
<br>
conclusions so far:
- the single-latent version achieves comparable quality
- an NF posterior does not improve anything, but an NF prior drastically improves uncond. generation
- some global properties are well encoded in the latent space in a controllable fashion
- encoded properties are spread over almost all dimensions

---
### Results - metrics

VCTK words:

| model      | mode           | KID    | FAD     | Vendi  |
| ---------- | -------------- | ------ | ------- | ------ |
| no-NF      | reconstruction | 0.0015 | 0.0248  | 5.9449 |
| NF         | reconstruction | 0.0015 | 0.0247  | 6.1620 |
| RAVE       | reconstruction | 0.0017 | 0.0268  | 5.9428 |
| no-NF      | unconditional  | 0.0534 | 0.5886  | 3.9058 |
| NF         | unconditional  | 0.0549 | 0.6049  | 3.5694 |
| RAVE+prior | unconditional  | 0.0535 | 0.5804  | 4.3253 |
| NF+prior   | unconditional  | 0.0115 | 0.1491  | 5.6436 |
| **self**   | **comparison** | 0.0000 | -0.0001 | 6.1212 |

<br>
Percussive:

| model    | mode           | KID    | FAD    | Vendi   |
| -------- | -------------- | ------ | ------ | ------- |
| **self** | **comparison** | 0.0000 | 0.0000 | 21.6521 |
| no-NF    | reconstruction | 0.0045 | 0.1569 | 15.2375 |
| NF       | reconstruction | 0.0040 | 0.1271 | 15.2508 |
| RAVE     | reconstruction | 0.0126 | 0.3082 | 16.4382 |
| no-NF    | unconditional  | 0.0537 | 0.8103 | 10.0454 |
| NF       | unconditional  | 0.0504 | 0.7732 | 10.2943 |
| RAVE     | unconditional  | 0.0350 | 0.7139 | 14.0476 |
| NF+prior | unconditional  | 0.0180 | 0.3539 | 9.7582  |

<br>
Sfx:

| model    | mode           | KID    | FAD    | Vendi   |
| -------- | -------------- | ------ | ------ | ------- |
| **self** | **comparison** | 0.0000 | 0.0000 | 39.0917 |
| no-NF    | reconstruction | 0.0066 | 0.1627 | 27.2088 |
| NF       | reconstruction | 0.0072 | 0.1754 | 26.9966 |
| RAVE     | reconstruction | 0.0115 | 0.2590 | 24.6911 |
| no-NF    | unconditional  | 0.0685 | 0.9353 | 4.6449  |
| NF       | unconditional  | 0.0693 | 0.9768 | 4.1123  |
| RAVE     | unconditional  | 0.0288 | 0.5165 | 18.2279 |
| NF+prior | unconditional  | 0.0217 | 0.3678 | 16.0714 |
