---
title: Thesis progress report 1
date: 2026-06-25
feed: show
category: posts
layout: Post
---
**Current status:**
- Extracted a "VCTK words" dataset from VCTK using Qwen3-ForcedAligner-0.6B
- Created a dataset of percussive sounds from random sample packs and the Laion630K Freesound subset
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
- All of the metrics are based on CLAP embeddings using the `music_audioset_epoch_15_esc_90.14.pt` model and the [official implementation](https://github.com/LAION-AI/CLAP)


<br>
Results for VCTK words:

| model         | mode           | KID    | FAD    | Vendi  |
| ------------- | -------------- | ------ | ------ | ------ |
| no-NF         | reconstruction | 0.0013 | 0.0203 | 5.2167 |
| NF            | reconstruction | 0.0014 | 0.0210 | 5.4041 |
| RAVE          | reconstruction | 0.0014 | 0.0205 | 5.3838 |
| no-NF         | unconditional  | 0.0765 | 0.7830 | 3.9264 |
| NF            | unconditional  | 0.0772 | 0.7977 | 3.7587 |
| RAVE+prior20M | unconditional  | 0.0737 | 0.7343 | 3.8908 |
| RAVE+prior3M  | unconditional  | 0.0670 | 0.6852 | 4.0574 |
| NF+prior      | unconditional  | 0.0138 | 0.1673 | 6.2118 |
| **self**      | **comparison** | 0.0000 | 0.0000 | 5.0779 |


<br>
Results for Percussive: