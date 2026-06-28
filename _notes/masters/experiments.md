on each of the [[datasets]]
# relevant work to consider
- [[RAVE]]
    - [x] test run training custom RAVE model on graz
- [[MelGAN]] decoder
    - test run training custom MelGAN model on graz
    - idk if this is necessary, many of the papers have just one previous work to compare against
# datasets
[[datasets]]

final choices:
- [[VCTK]] words
- percussive
- (TODO) sth wonky
# experiments
connected to the narrative described in [[thesis goals]]
### reconstruction
1. [[Frechet Audio Distance]] w/ CLAP embeddings vs val set
2. [[Mean Opinion Score]]
### unsupervised generation
(implicitly) quality/diversity of the generated samples
- FAD vs val set
- KID vs val set

[[diversity metrics]]:
- [[The Vendi Score]]
### latent space analysis
1. label examples with perceptual categories, do dimensionality reduction in the latent space
    - quantitative: measure clustering quality of this labeling in the latent space (silhoutte metrics)
    - quantitative: classifying the features based on the latent space or PCA transformed space (_linear latent arithmetic_)
    - qualitative: just a visual evaluation of these clusters
2. modifying a single sample in some latent space direction or interpolate between different samples and "visualize" the transformation (like drawing a mnist grid) (_interventional travels_)
3. meaningful dimensions - "effective" dimensionality, like in [[RAVE]]
4. a measure of the quality of the latent space - [gilbo](https://arxiv.org/abs/1802.04874)
    1. mutual information between latent and data distributions
    2. plan with a small description - `~/.claude/plans/hi-there-so-i-tingly-newt.md `
    3. "how many distinguishable samples does this decoder actually produce as a function of its latent input?"
5. latent space used dimensions - _fidelity_ from [[RAVE]]

disentaglement metrics?
- [[Mutual Information Gap]]
- DCI (Disentanglement, Completeness, Informativeness)
- Separated Attribute Predictability

fun experiment but idk what would the conclusions be: take the same word from different speakers/context -> see what latent space representations they have -> interpolate between them
### ~~single latent reduction method~~
inter-channel dependencies or not? yeah probably safe to say they're necessary
maybe a different option would be several more conv layers instead of the extreme `kernel_size=24` or sth ok maybe that's not that extreme
### ~~ablation studies~~
candidates
- normalization
- amp mod
- noise gen
- discriminators
- activations
- normalizing flows
- single latent
ok not too much bro
# comparisons
### single vs. sequence latent space
so basically comparing w/ [[RAVE]]
### single latent vector w/ normalizing flows
different variations:
- prior learned from the start vs second stage
- with/without posterior flow
- with/without prior flow

Q: do they
- improve reconstruction
- unsupervised generation
- disentaglement
and at what param count increase cost?
basically other evaluations comparing models with/without NFs

ideal version would be drawing the relationship between additional param count and other metrics

out of scope: comparing different normalizing flows
# specific experiments to run
- VCTK words
    - [x] (vctk) train no-NF
    - [x] (vctk) train NF
    - [x] (vctk) train RAVE+prior
    - [x] (vctk) train NF prior
- percussive2
    - [x] (percussive2) train no-NF
    - [x] (percussive2) train NF
    - [ ] (percussive2) train RAVE+prior
    - [x] (percussive2) train NF prior
- freesound_sfx
    - [ ] (freesound_sfx) train no-NF
    - [ ] (freesound_sfx) train NF
    - [ ] (freesound_sfx) train RAVE+prior
    - [ ] (freesound_sfx) train NF prior

the "eval suite" is:
- reconstructions -> FAD/KID vs val set (_quality_)
- uncond. generations -> FAD/KID vs val set (_quality_)
- uncond. generations -> Vendi score (_diversity_)
- encode(val_set) -> (dim reduction?) -> clustering w/ percep. labels -> silhouette metrics (_disentaglement_)
- encode(val_set) -> linear classification of percep. labels  (_disentaglement_, _controllability_)
- encode(val_set) -> dim reduction -> MIG based on percep. labels (_disentaglement_)
- interpolate two latents -> decode (_idk even_)
- modify a single latent in different directions -> decode (_idk even_)
- gilbo (data-latent mutual information - _learned model complexity_)
- posterior-prior mmd (prior holes - _prior quality_)

and visualizations?
- for NF models - graph the prior somehow
