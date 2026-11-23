2020 - J. Nistal
[paper](https://arxiv.org/abs/2008.12073) (updated 2022?)
[repo](https://github.com/SonyCSLParis/DrumGAN)

[drumgan vst paper](https://arxiv.org/pdf/2206.14723) - a 2022 update making it 44.1kHz for the steinberg plugin
[backbone 1.5](https://cslmusicteam.sony.fr/news/steinberg-releases-backbone-1-5-incorporating-drumgan/)

uses an Audio Commons Timbre model
new - generates STFT _with_ the complex part, audio is then just the inverse STFT

the net is conditioned on labels which then allows conditional generation, mixing classes (snare+kick) etc
trains an unconditioned net for comparison which is kind of my case?
# diversity discussion
"Therefore, it is interesting to see that rand feats cause outputs which overall do not follow the distribution of the real data (i.e., high KID), but the individual outputs are still plausible percussion samples (i.e., low FAD)"
- oh shit that's a diversity/novelty metric right there?
# dataset
info:
- 300k samples
- balanced across kicks, snares and cymbals
- cut to 1s each
- downsampled to 16kHz
# goals/evaluation
### experiments
- evaluating "controllability" (disentaglement?) based on methods from [this paper doing conditioned percussive sound generation too](https://arxiv.org/pdf/1911.11853)
    - done via the same feature extractors as for training
### evaluation methods
- [[Frechet Audio Distance]]
    - reference set? "real data", not training data I presume
- Inception Score with custom Inception classifier
- Kernel Inception Distance
    - measures distance between real and generated distributions
    - based on the last layer of a custom Inception model
### narrative
the key goal outlined in the abstract is synthesis done by controlling _high-level_ meaningful features rather than low-level ones, like in traditional synthesizers
- shi same thing
this can be done with a model conditioned on relevant information
the authors train a GAN conditioned on perceptual features from a publicly available feature extractor, which enables intuitive and high-level control over the generation process

what do they show?
- increased quality vs the baseline U-net approach ("specific prior work")
- the conditional input shapes the sound characteristics

how could I continue this narrative?
- rather than rely on feature extractors, a model can extract high-level features itself
    - shit a comparison of learned-vs-extracted features would be interesting on percussive
# reproduction
`train.py` script
needs a config w/
- custom loader
- model architecture
ok thx Claude
installed additionally in `mgr-rave`:
```
rich, dill, ipdb, mdct, plotly, matplotlib, toolz
```
fixed some call sites
to train:
```
conda activate mgr-rave
cd /home/jp/mgr2024/DrumGAN
mkdir -p output_networks   # mkdir_in_path is non-recursive; parent must exist
python train.py PGAN -c config_files/vctk_words.json --no-visdom
```