J. Nistal (2020)
[paper](https://arxiv.org/abs/2008.12073) (updated 2022?)
[repo](https://github.com/SonyCSLParis/DrumGAN)

uses an Audio Commons Timbre model
new - generates STFT _with_ the complex part, audio is then just the inverse STFT
trains an unconditioned net for comparison which is kind of my case?
# diversity discussion
"Therefore, it is interesting to see that rand feats cause outputs which overall do not follow the distribution of the real data (i.e., high KID), but the individual outputs are still plausible percussion samples (i.e., low FAD)"
- oh shit that's a diversity/novelty metric right there?
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
### discussion
