https://openreview.net/forum?id=HkpbnH9lx
https://arxiv.org/abs/1605.08803
https://arxiv.org/pdf/1605.08803

continuation of [NICE](https://arxiv.org/abs/1410.8516)

- [ ] why Real NVP, not other flows?
## so
the idea is to have a class of _bijective_ generative models from some latent space to the target distribution, which would allow maximum likelihood learning without discriminator networks (GANs) or approximate posterior inference (VAEs)

being bijective (invertible) enables likelihood calculation using the change of variables formula:
![[Pasted image 20251229115252.png]]
in general the determinant is infeasible (matrix inversion + determinant), but we can use (as in NICE) the property that the determinant of a triangular matrix is just the _product of the diagonal_

the transform itself is a translate+scale on half of the input based on the other half, the halfs alternate
translate/scale is done with neural networks
![[Pasted image 20251229211527.png]]