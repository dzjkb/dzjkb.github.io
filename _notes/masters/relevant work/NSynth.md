2017 - Engel
[paper](https://arxiv.org/pdf/1704.01279)
- basically encoding everything except pitch in temporal embeddings
- trained on insturments playing single notes
- temporal -> capture local context, not over time
- gives e.g. style transfer
## training
trained on log-magnitude spectra?
with MSE
does this mean it's like an implicit spectral loss?
## evaluation
### experiments
- qualitative - spectral graphs w/ instantenous frequency
- quantitative - a classifier trained to reconstruct pitch + quality from the audio
    - quality - tags like "dark", "bright"
- entaglement of pitch and timbre - classifying pitch from the timbre vector, correlation graphs