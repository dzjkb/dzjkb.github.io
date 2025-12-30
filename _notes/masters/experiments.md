on each of the [[datasets]]
# compare against
- [[RAVE]]
    - [x] test run training custom RAVE model on graz
- [[MelGAN]] decoder
    - test run training custom MelGAN model on graz
    - idk if this is necessary
# experiments
### reconstruction
just **quality** on several datasets
metrics:
- ~~FAD~~???? TBD
- MOS
### single vs. sequence latent space
### unsupervised generation
reference-free quality of the generated samples
- FAD

**diversity**??? [[diversity metrics]]
### single latent vector w/ normalizing flows
do they
- improve reconstruction
- unsupervised generation
- disentaglement
### latent space analysis
showing disentaglement
- has to be qualitative - sets of samples with a clear singled out "feature"
meaningful dimensions
- "effective" dimensionality, like in [[RAVE]]
### ablation studies
candidates
- normalization
- amp mod
- noise gen
- discriminators
- activations
- normalizing flows
- single latent
ok not too much bro
most important:

**normalizing flows + single latent**
with/without prior flow
- important from the perspective of unconditional generation
- keeping prior+decoder size constant?
prior learned from the start vs second stage
with/without posterior flow
with time series latent
- can't do unconditional generation here
- although maybe I should, to compare with [[RAVE]] unconditional generation
