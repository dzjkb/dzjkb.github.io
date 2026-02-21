2021 - Rouard
https://arxiv.org/abs/2106.07431
[repo](https://github.com/simonrouard/CRASH)
[examples](https://crash-diffusion.github.io/crash/)

what?
- diffusion on short 44.1kHz drum samples
- conditional U-Net to approximate the "score function" (?)
- enables unconditional generation, class-conditioned generation, inpainting etc.

```
By using the latent representation given by the forward Ordinary Differential Equation, you can also load any 44.1kHz drum sound and manipulate it.
```
does this overcome the limitation of density estimation for diffusion??
- is that a real limitation? I'm assuming it's similar to GANs

goddamn the examples are good
- [x] do they have an open dataset???
![[Pasted image 20251230220736.png]]
nope rip
same ds as [[DrumGAN]], both works are from Sony CSL ok that would make sense
# evaluation
- [ ] what are the evaluation methods? quality, diversity?