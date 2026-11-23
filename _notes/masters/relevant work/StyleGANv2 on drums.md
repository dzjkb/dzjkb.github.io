[repo](https://github.com/aframires/stylegan2-ada-pytorch) (from the paper below, forked from the official nvidia stylegan 2 impl)
[stylegan 2 on drum sounds (paper)](https://dafx2020.mdw.ac.at/proceedings/papers/DAFx20in22_paper_22.pdf)

runs on spectrograms
hyperparams - ds:
- 20630 samples (not that different from perc2)
- 16kHz sr, 1s audio with zero padding if necessary
- logarithmic fade-out for the last 30% of each sample
- 1024x32 spectrograms
- complex STFT with hop=512, window_size=2048, fft_size=2048
    - complex spectrograms are reshaped into a 2-channel feature map
- DC component removed from the spectrograms
model:
- smaller generator feature map set to 64x2, doubling every layer
- modified to handle only 2 channels
- 5 synthesis blocks in the generator comprising a Modulated Convolutional and an Upsampling layer
- discriminator 5 blocks comprising of a Convolutional and a Downsample layer
- latent & style space size 512
training:
- lr 2e−3
- batch size 8
- early stopping with FAD

applies both supervised and unsupervised feature extraction
- supervised: [InterfaceGAN](https://arxiv.org/abs/1907.10786) - 7 binary features with a hyperplane separating each in the latent space, generate samples and find the hyperplane with an SVM
- unsupervised: [SeFa](https://genforce.github.io/sefa/)
# prior work
- DrumGAN
- Adversarial Synthesis of Drum Sounds
# goals
>We aim to evaluate how different interfaces support creative control over drum generation by conducting a user study based on the Creative Support Index.

evaluating 3 different approaches to controlling the latent space during generation
# evaluations
#### Creative Support Index

| Interface    | CSI score     |
| ------------ | ------------- |
| SeFa         | 66.99 ± 11.38 |
| Z-Space      | 62.85 ± 11.91 |
| InterfaceGAN | 62.18 ± 11.40 |
#### Debriefs with participants
The SeFa directions seemed clearly related with sound concepts:
- the first slider controlled the class of the drum sound
- second: noise content
- later: decay, depth, boominess
- sliders 5/6 were less consistent across different samples
InterfaceGAN had less interpretability and seemd less orthogonal
# future work
>Redoing the experiment in a more specific scenario (e.g. replicating a drum sound or exploring drum sounds to fit a composition) could lead to a more confident result.
# comments
what can I use this for?
- argument this is why I'm comparing against SeFa rather than the supervised methods

how can I extend this?
- new sounds still valid
- VAEs provide better latent space directions - in what way?
    - more of them? more interpretable?
    - needs the baseline trained