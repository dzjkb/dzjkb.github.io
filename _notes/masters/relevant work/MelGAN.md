2019 - Kumar (Lyrebird AI)
- https://www.descript.com/lyrebird - voice synthesis, cloning etc.
- https://www.descript.com/ - AI video editing
- jfc

[original paper](https://arxiv.org/pdf/1910.06711)
[code](https://github.com/descriptinc/melgan-neurips)
basing on (or at least after):
- WaveNet
- WaveGAN
- Tacotron
- some deep voice shit or sth
- GANSynth
### contributions
text to speech from mel spectrograms
non-autoregressive convolutions
- or is it? parallel wavenet from Oord himself did it in 2018 no?
- or at least [[Parallel WaveGan]] did, seems like these happened at the same time
Unlike GANSynth, modeling the waveform instead of STFT magnitudes (which then required an additional step of phase estimation and reconstruction)
### architecture
### misc
uses 0.5, 0.9 betas for Adam (RAVE inspiration?)
finds weight normalization looks best (RAVE inspiration again?)
does LSTM prior learning on sequences from the latent space! (RAVE inspiration def)
![[Pasted image 20250928141744.png]]
# goals/evaluation methods
- [x] what are the goals/evaluation methods for MelGan?

### experiments
- ablation studies (MOS)
- comparing text to speech with different text->melspec backends (MOS)
- VAE style music reconstruction (only qualitative analysis [here](https://melgan-neurips.github.io/)) (showing generality of the approach)
- VQ-VAE music reconstruction
    - original [from Oord too](https://arxiv.org/abs/1711.00937)
    - the latent representation is a sequence of discrete vectors **plus** a global latent vector, interesting
    - latent vectors are in the place of mel spectrograms
### evaluation methods
**Mean Opinion Score** tests (described in appendix C)
- 200 individuals
- each one ranks 15 samples from 1 to 5 on headphones

**Qualitative analysis**
- basically a couple of samples on a webpage
- supplements the quantitative analysis above