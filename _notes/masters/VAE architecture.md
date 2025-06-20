# References
references: VAE with flows, RAVE, SoundStream
- [x] find more references for encoder/decoder architecture choices
# Architecture
choices
- same arch as RAVE/SoundStream
- no MFCCs because non-harmonic audio
- no PQMF for now
- simple `/stride` and `*stride` down/upsampling
- non-causal (i.e. classical) convolutions
- short fixed-length audio
# Training
choices
- try no discriminator/quality fine-tuning first, hopefully isn't necessary in our case