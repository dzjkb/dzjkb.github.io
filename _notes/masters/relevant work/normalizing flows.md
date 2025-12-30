
iconic
![[Pasted image 20250323132611.png]]


## theory
invertible transforms
more expressive latent space -> better reproduction/unconditional generation
- isn't this just equivalent to more capacity? the flow could be just learned by the decoder

#### [Variational Inference with Normalizing Flows](https://arxiv.org/pdf/1505.05770)
```
It is one of these limitations, the choice of
posterior approximation, that we address in this paper.
```
what about the priors?

```
An important conclusion from the discussion in section 3
is that there exist classes of normalizing flows that allow us
to create extremely rich posterior approximations for vari-
ational inference. With normalizing flows, we are able to
show that in the asymptotic regime, the space of solutions
is rich enough to contain the true posterior distribution
```
right that's exactly what I want, no?
- [ ] Variational Inference with normalizing flows - what class of flows to approximate the posteriors is this?

DLGMs - generalizations of models with latent variables, apparently [[VAE]]s can be thought of as DLGMS 

alternative KL divergence (_free energy_) formulation is totally different, need to find an implementation of this

#### [Normalizing Flows for Probabilistic Modeling and Inference](https://arxiv.org/pdf/1912.02762)
2021 - "unified perspective" on the field
"We place special emphasis on the fundamental principles of flow design, and discuss foundational topics such as expressive power and computational trade-offs"
nice
## works
[[WaveGlow]]
ohboy
based on [glow](https://arxiv.org/pdf/1807.03039) (images) and [[WaveNet]]
conditioned on spectrograms, transforms a gaussian (of the same length as the desired output?) into audio using flows
doesn't seem like the usage I was going for (the flow isn't applied to the posterior/prior)

[Conditional Variational Autoencoder with Adversarial Learning for End-to-End TTS - VITS (2021)](https://arxiv.org/pdf/2106.06103)
"we apply normalizing flows to our conditional prior distribution" ??????????
right, the flow is _not_ used for the posterior, only for the prior, huh
but the prior is also a _conditional_ prior, based on phoneme embeddings
and then the KLD is log posterior minus log prior, ok
"The normalizing flow is a stack of affine coupling layer"
LJ speech and VCTK datasets

flows - affine coupling layers with f-king WaveNets hmmm

[Naturalspeech: End-to-end text to speech synthesis with human-level quality (2022)](https://arxiv.org/abs/2205.04421)
"We design a bidirectional prior/posterior module based on flow model"
yissss
ok but it's also a _conditional_ prior based on a phoneme sequence
- still, maybe just optimizing a flow prior starting from `N(0, 1)` might make sense?
ye idk man cool but I'm not sure this is relevant

[WaveFlow (2020)](https://arxiv.org/abs/1912.01219)
yeah this is a continuation of WaveGlow, so no

[Improving Conditional VAE with approximation using Normalizing Flows](https://arxiv.org/pdf/2511.08946)
some random noname paper
again, using flows for conditional VAE

https://neurips.cc/virtual/2025/loc/san-diego/poster/118418
segmentations using GMM for latent space + NFs + CVAE
"GMM structures the latent space into meaningful semantic clusters" hmmmm??

https://pyro.ai/examples/vae_flow_prior.html
ok ok ok lesgo
using a MAF for the prior, yes

[Normalizing Flows: An Introduction and Review of Current Methods (2019)](https://arxiv.org/abs/1908.09257)
mmmkay might come in handy

[Variational Autoencoders with Normalizing Flow Decoders (2020)](https://arxiv.org/pdf/2004.05617)
oh shi
```
Such a model was originally proposed in [Chen et al.,
2017], where the authors show that it is equivalent to Inverse
Autoregressive Flow (IAF) [Kingma et al., 2016] along the
encoder path, while being deeper along the decoder path. We
therefore obtain the benefit of using a flexible posterior while
simultaneously being able to close the gap between the en-
coder and prior distributions. We found in our experiments
that by using a normalizing flow prior we were able to obtain
better FID scores than by using a second-stage VAE. Results
are shown in the experiment section.

In our implementation we use a normalizing flow similar
in structure to Real NVP [Dinh et al., 2016] (which is a spe-
cial case of autoregressive normalizing flows [Papamakarios
et al., 2017]), as it allows both efficient training and sampling
```
ok ok ok
not that much detail on the VAE w/ NF training
but there's NF applied to priors and that's the key

[Learning Energy-based Variational Latent Prior for VAEs (2025)](https://arxiv.org/html/2510.00260)
oh shi this is fresh
```
Ideally, a prior needs to be flexible enough to match the posterior while retaining the ability to generate samples fast.
```
huh, other way around indeed -> prior modeling posterior outputs like in the [[RAVE]] prior
`alternating optimization problem` for prior optimization? hmmm
references [A Contrastive Learning Approach for Training Variational Autoencoder Priors (2020)](https://arxiv.org/abs/2010.02917) working with images
also uses Real NVP, FID and KID, niceee
similar to [[RAVE]] again learns the prior in a second stage of training, uses isotropic gaussian for the first stage
- lists other 2-stage prior training methods
```
The first result from Proposition 1 says that our formulation of EBM-based prior is equivalent to training a WGAN in the latent space of the VAE, with an assumption of restricting the EBM class to 1-Lipschitz functions.
```
tf

"energy based" prior, which utilizes a normalizing flow as one of the components
but I'd just go straight for an NF prior

uses nearest neighbors in PCA-transformed latent space to assess overfitting hm ok ok
MMD to measure "prior holes", so pretty much the difference of prior/posterior distributions
KLD weight experiment would suggest that even if we set it low (i.e. optimizing for reconstruction rather than generation), we still get good generation quality (measured by FID), which would suggest the decoder doesn't learn the more complicated prior itself
## other reading
- [x] read [Conditional Variational Autoencoder with Adversarial Learning for End-to-End TTS - VITS (2021)](https://arxiv.org/pdf/2106.06103)
- [x] read [Naturalspeech: End-to-end text to speech synthesis with human-level quality](https://arxiv.org/abs/2205.04421)
- [ ] [Improving Variational Inference with Inverse Autoregressive Flow](https://arxiv.org/abs/1606.04934)
    - idk this is an old one
- [ ] [Learning Energy-based Variational Latent Prior for VAEs](https://arxiv.org/html/2510.00260)
- [x] [Variational Autoencoders with Normalizing Flow Decoders (2020)](https://ar5iv.labs.arxiv.org/html/2004.05617)
    - https://arxiv.org/pdf/2004.05617
- [x] FloWaveNet (2019)
- [x] [WaveFlow (2020)](https://arxiv.org/abs/1912.01219)
## implementations
https://github.com/VincentStimper/normalizing-flows
![[Pasted image 20251220155102.png]]
yeah so [this VAE example](https://github.com/VincentStimper/normalizing-flows/blob/master/examples/vae.py) is what I'm going for right????
simple fully connected encoder -> decoder + flows transforming `z` _after_ reparametrization
doesn't do unconditional generation

https://github.com/fmu2/flow-VAE
- wait so the flows are only used for the posterior not the prior?? `sample()` goes from sampling `z ~ N` straight into the decoder
- flows using to approximate the _posterior_ would suggest this is intentional, not using flows for the prior
"Based on the experiments, I concluded that the idea of enriching approximate posteriors in VAE using normalizing flows is theoretically appealing but can easily cause more headache than benefit in real practice."
mmmmmhm

there's also this [vae_nf](https://github.com/abhisheksaurabh1985/vae_nf/blob/master/losses.py) repo but w/e

https://github.com/probabilists/zuko
? sure

https://pyro.ai/examples/intro_long.html
oh shi oh shi, some big probabilistic ML package
or wait no it uses zuko for the NFs nvm, but
![[Pasted image 20251224112904.png]]
!!!!