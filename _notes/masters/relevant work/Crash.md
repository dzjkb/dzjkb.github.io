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

0.5s samples maybe that's a better idea?
# evaluation
- [x] what are the evaluation methods? quality, diversity?

training for 120 epochs with a checkpoint every 8 epochs
- every checkpoint 2700 generated sounds are saved
- afterwards, the best checkpoint is chosen and 27000 sounds are generated
    - for each of the different generation methods
- [[Frechet Audio Distance]] is calculated between the generated sounds and the test set
    - FAD is also calculated against the test set with 10e-4 gaussian noise added since it's imperceptible
- only FAD is reported for quantitative evals

diversity:
```
Finally, all models generate kicks, snares and cymbals in
equal proportions but the generated samples are a bit less
diverse than in the original dataset.
```
just qualitative evaluation it seems
```
The relative lack of diversity of the unconditional generation
is not dramatic since the model can still perform interac-
tive sound design by modifying existing samples from the
dataset
```

other experiments:
- noising/denoising samples
- inpainting sounds
- class-conditioning, class-mixing based on a kick/snare/cymbal classifier
# misc
FAD between the test set and the same set but with 10e-4 gaussian noise added is 0.72 ?????
- "FAD is a very sensitive metric"
- shit, but it's probably the original VGGish embeddings