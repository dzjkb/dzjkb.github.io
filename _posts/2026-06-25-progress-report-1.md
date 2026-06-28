---
title: Thesis progress report 1
date: 2026-06-25
feed: show
category: posts
layout: Post
---
**Current status:**
- Extracted a "VCTK words" dataset from VCTK using Qwen3-ForcedAligner-0.6B
- Created a dataset of percussive sounds from random sample packs and a Laion630K Freesound subset
- Trained 3 VAEs on each dataset:
    - without normalizing flows
    - with a normalizing flow posterior
    - RAVE v2 from the original implementation
- and 2 priors:
    - an NF prior for the NF posterior model
    - a RAVE prior (tested two sizes: original 20M, reduced 3M to fit the audio length)

<br>
**VCTK words dataset:**
- 225748/24872 train/val samples
- each sample is a ~1s clip with one word
- most words are substantially shorter than 1 second, but words below 0.1s were filtered out

<br>
**Percussive dataset:**
- 13452/1511 train/val samples
- same ~1s length
- mostly snare/kick/clap/rimshot sounds

<br>
**Notes on metrics:**
- FAD is calculated in the FAD-inf version as defined in [Adapting Frechet Audio Distance for Generative Music Evaluation (arxiv)](https://arxiv.org/abs/2311.01616)
- KID is based on the [DrumGAN implementation](https://github.com/SonyCSLParis/DrumGAN/blob/master/evaluation/metrics/maximum_mean_discrepancy.py)
- [The Vendi score](https://arxiv.org/abs/2210.02410) is calculated using the [official implementation](https://github.com/vertaix/Vendi-Score)
- I decided not to use precision/recall metrics, which are sometimes used to evaluate diversity, since I believe they would be redundant with KID + FAD + Vendi
- All of the metrics are based on CLAP embeddings, with `music_speech_epoch_15_esc_89.25.pt` used for VCTK and `music_audioset_epoch_15_esc_90.14.pt` used for the rest

<br>
**further eval ideas:**
- 3rd dataset - something like an SFX set maybe?
- disentaglement evaluation using perceptual labels on VCTK:
    - clustering with silhouette metrics
    - mutual information gap
    - training linear classifiers on the latent space
- qualitative analysis:
    - interpolating between two latents
    - translating a given latent in various directions

---
### Results
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
| no-NF    | reconstruction | 0.0045 | 0.1569 | 15.2375 |
| NF       | reconstruction | 0.0040 | 0.1271 | 15.2508 |
| RAVE     | reconstruction |        |        |         |
| no-NF    | unconditional  | 0.0537 | 0.8103 | 10.0454 |
| NF       | unconditional  | 0.0504 | 0.7732 | 10.2943 |
| RAVE     | unconditional  |        |        |         |
| NF+prior | unconditional  | 0.0180 | 0.3539 | 9.7582  |
| **self** | **comparison** | 0.0000 | 0.0000 | 21.6521 |

<br>
Fidelity for VCTK words, as defined in RAVE:

| fidelity | no-NF | NF  |
| -------- | ----- | --- |
| 0.8      | 53    | 106 |
| 0.9      | 77    | 133 |
| 0.95     | 102   | 151 |
| 0.99     | 154   | 181 |


---
### Thesis outline