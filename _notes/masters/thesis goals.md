side note: https://typst.app/
# primary
overall goals:
- high quality, diverse sample generation - 2-3s clips with music production in mind
- a tool (VST potential) for music production
    - kind of a [[RAVE]] rip-off again, my og idea though
and main contributions?
- new datasets - samples, synths
    - specific for textures/instruments (like percussives)
- hopefully - higher quality due to different constraints
- potentially - normalizing flows or other modifications that improve _quality_ or _latent space properties_
### reconstruction quality - quantitative eval 1
listed in [[quality metrics]], [[experiments]]
need to
- [x] choose models to perform comparison against
- [x] choose a couple of metrics and go with it

optimizing **spectral loss** and **discriminator loss**
measuring **FAD**, **KID** on the test sets
### diversity, novelty - quantative eval 2
listed in [[diversity metrics]], [[experiments]]
- [x] describe conceptually why do we want diverse samples and what does it mean
- [x] how can I evaluate diversity of generated samples?
### new kind of dataset
and possible method changes to accomodate the new problem
- Irisarri
- percussive sounds
- sound fx stuff - glitches?
    - loads of packs to work off of too, probably
- Hecker
- [x] prep a classical piano (debussy+chopin) dataset (moved from [[2025-06-25]])

[[MusicLM]] also lists extending previous methods to a wider music domain as a contribution
## secondary
### meaningful latent space
any ways to actually shape it? try forcing a dimension to correspond to some abstract concepts (intensity? color? texture?) and see what happens?
- sounds like that guys phd thesis [[2025-12-10]]
disentaglement evals in [[experiments]]
### architecture improvements
improving VAEs, probably mainly through making the pior/posterior more sophisticated? [[normalizing flows]]

~~maybe by adding new DDSP components compared to RAVE?~~
# non-goals
- real-time generation
- usable modules? at least for now, would be nice to do later
# goal #2 - diversity
One of the goals of this thesis is to explore, for the studied methods, their capabilities to generate diverse and novel sounds. While this is often an overlooked aspect in generative audio models - usecases such as audio codecs or prompt-conditioned generation do not require novelty in the model itself - diversity is critical to creative applications. The artistic process is often one of exploration and experimentation, where the artist doesn't just impose his vision onto his means of expression - they come into dialogue with their tools, seeking inspiration in their unexpected behaviour or various quirks and building on them further.
For this reason the thesis places a larger emphasis on the diversity of unconditional generation with both qualitative and quantitative evaluation - see [[diversity metrics]].
# narrative
## 0 - a single latent vector enables novel  ways of creative usage
advantages:
- latent dimensions can capture global features
- latent dimensions can be modified to manually adjust
## 1 - audio quality is similar
_If we reduce the 23Hz continuous latent vectors down to one vector of length 256 (effectively increasing the compression ratio by TODO) we can still reconstruct high quality audio_

setup: standard training

metrics to show:
- FAD/KID on mgr reconstructions
- FAD/KID on RAVE reconstructions
- (?) FAD from the [[RAVE]] paper
## 2 - Normalizing Flows improve quality
_Using Normalizing Flows for posterior/prior distributions results in higher quality reconstructions_

setup: training w/ NFs

metrics to show:
- FAD/KID on mgr reconstructions
- [x] sth to show that the distributions are "better"? gilbo? clustering w/ perceptual labels?
- latent space [[experiments]] - ?
## 3 - Normalizing Flows improve prior generation
_Using Normalizing Flows for posterior/prior distributions results in generation from the prior_

setup: training w/ NFs

metrics to show:
- FAD/KID on mgr unconditional generations
- latent space [[experiments]] - ?
## 4 - the model learns disentangled features
_As above_

setup: standard training or NF trianing

graphs to show:
- viz in 3D with colors for labels
- disentaglement metrics, see [[experiments]] - ?
## ? - prior generation is easier with similar quality
_Unconditional generation from the prior doesn't require a specially trained model while retaining good quality_