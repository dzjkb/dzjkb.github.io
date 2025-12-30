The latent space regularization term explodes - why?
## References
- https://datascience.stackexchange.com/questions/48962/what-is-posterior-collapse-phenomenon
- [Learning Dynamics in Linear VAE](https://arxiv.org/abs/2310.15440)
    - nice description of disentaglement, will probably need to write something similar
    - nice mathematical analysis of the linear case (both the VAE and data being formulated as something like `Wx`)
    - but I'm not sure it's useful to me - conditions for optimal beta are derived, but I can't apply them to my data
- [Posterior Collapse and Latent Variable Non-identifiability](https://arxiv.org/abs/2301.00537)
    - proposes VAE modifications, dated 2023, could be interesting actually using this
- [Don't Blame the ELBO! A Linear VAE Perspective on Posterior Collapse](https://arxiv.org/abs/1911.02469#)
## Thoughts