**note**: these make sense only if we're talking about _unconditional_ generation

not sure actually if there were any examples in related works?
might have to come up with something myself
- something like the std of generated samples?

- [x] see if any other of the related works try to evaluate "diversity" of their outputs
- [ ] come up or find ways to evaluate diversity
    - maybe take inspiration from something in the image domain?

shit I'm not sure there's anything other people could come up with than these distances
## related
- [[MusicLM]] has a "diversity" section on the [examples website](https://google-research.github.io/seanet/musiclm/examples/), but it's just qualitative evaluation of several genertaed examples with fixed tokens

**stuff from different domains**
- [Adaptive input-image normalization for solving the mode collapse problem in GAN-based X-ray images](https://www.sciencedirect.com/science/article/abs/pii/S1746809425008444)
- [DivGAN: A diversity enforcing generative adversarial network for mode collapse reduction](https://www.sciencedirect.com/science/article/abs/pii/S0004370223000097)
    - [pdf](https://drive.uqu.edu.sa/up/forms/gssr/37808/848856/research_path_1675237873_knlwPNMGX5.pdf)
    - so - adds another auxiliary network (like the discriminator) that measures diversity and is included in the loss term
    - embeddings + distance (yeah sounds familiar)
    - meh
- [dp-GAN : Alleviating Mode Collapse in GAN via Diversity Penalty Module](https://arxiv.org/pdf/2108.02353v1)
    - ok - enforcing different input noise vectors to results in proportionally different outputs, basically making the GAN more "injective"
    - not sure that applies? maybe
- [Diversity-Sensitive Conditional GANs](https://arxiv.org/pdf/1901.09024)
    - also adds a regularization term - distance of generated images over distance of latent input vectors
## concept
why do I want diverse samples for music production anyway? the point is to have an "instrument" with meaningful (or not, nice to have) parameters I can tweak
is it bad if they're not diverse? if they're not diverse, they're probably similar to the training set which defeats the purpose of the model altogether
or maybe the purpose is really the other things I get (transfer, meaningful latent space) instead of the things I can quantitatively measure - similar to [[RAVE]]
so maybe it wouldn't defeat the purpose but diversity _could_ be one of the achievements if it's good
## ideas
something based on distance metrics - e.g. [[Frechet Audio Distance]], in embedding space
distance can give us a measure of "dispersion"
- possible inspiration - measures that halton sequences try to maximize?
- discrepancy - I can literally just calculate discrepancy in an embedding space
    - isn't that what [[Kernel Inception Distance]] does
- taking from MSFT's [[Frechet Audio Distance]] paper - L-CLAP or CLAP should be good embeddings
    - from the [FAD toolkit](https://github.com/microsoft/fadtk) to be precise
- but I could also study serveral and see what qualities they capture, or just compare results between several of them
- would be great to find a paper doing something similar, look at papers that evaluate unconditional generation
    - [[DrumGAN]]
- [paper proposing halton sequences for hyperparam search](https://arxiv.org/pdf/1706.03200), [low-discrepancy sequences (wiki)](https://en.wikipedia.org/wiki/Low-discrepancy_sequence)
- embeddings discrepancy - or other measures? max distance or sth
- but also comparing the embeddings positionally to the training set embeddings !!