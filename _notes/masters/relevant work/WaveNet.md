2016 - Van den Oord
[paper](https://arxiv.org/pdf/1609.03499)
dilated + causal convolutions, thank wavenet
# goals/evaluation methods
- [x] what are the goals/evaluation methods of WaveNet?
baseline - concatenative speech generation
### experiments
- unconditioned speech generation - english, mandarin (VCTK)
- text to speech - english, mandarin, MOS
- unconditioned piano music generation
    - just qualitative analysis
    - dataset - 60hrs of piano from youtube
- speech recognition
### evaluation methods
**Mean Opinion Score**
- 2 samples - preference or neutral
- additionally - rate the "naturalness" in a five-point Likert scale
- described in Appendix B