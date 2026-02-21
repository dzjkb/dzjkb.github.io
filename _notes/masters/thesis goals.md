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
listed in [[quality metrics]]
need to
- [x] choose models to perform comparison against
- [x] choose a couple of metrics and go with it

optimizing **spectral loss** and **discriminator loss**
measuring **FAD**, **KID** on the test sets
### diversity, novelty - quantative eval 2
listed in [[diversity metrics]]
- [ ] describe conceptually why do we want diverse samples and what does it mean
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
### architecture improvements
improving VAEs, probably mainly through making the pior/posterior more sophisticated? [[normalizing flows]]

maybe by adding new DDSP components compared to RAVE?
# non-goals
- real-time generation
- usable modules? at least for now, would be nice to do later