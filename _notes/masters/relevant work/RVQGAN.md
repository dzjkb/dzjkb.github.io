2023 - Kumar (descript)
[paper](https://arxiv.org/pdf/2306.06546)
[repo](https://github.com/descriptinc/descript-audio-codec)
with a CLI to encode/decode audio lessgooo
# architecture
why weight norm not spectral norm for the discriminators?
- nothing in the paper

`[2, 4, 8, 8]` downsampling rates in the encoder
# goals/evaluation
### experiments
-
### evaluation methods
- [[ViSQOL]]
- Mel distance
- spectral (STFT) distance
    - "This metric captures the fidelity in higher frequencies better than the mel distance"
- Scale-invariant source-to-distortion ratio (SI-SDR)
- Bitrate efficiency
- MUSHRA
```
We also conduct a MUSHRA-inspired listening test, with a hidden reference, but no low-passed anchor. In it each one of ten expert listeners rated 12 randomly selected 10-second samples from our evaluation set, 4 of each domain; speech, music and environmental sounds. We compare our proposed system at 2.67kbps, 5.33kbps and 8kbps to EnCodec at 3kbps, 6kbps and 12kbp
```