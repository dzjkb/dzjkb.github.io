from [[DrumGAN]]
their [implementation](https://github.com/SonyCSLParis/DrumGAN/blob/master/evaluation/metrics/kid.py)
using formulas from [Closed-form Expressions for Maximum Mean Discrepancy with Applications to Wasserstein Auto-Encoders](https://arxiv.org/pdf/1901.03227)

impl for standard KID in `torch_fidelity`
- not sure I could use custom models there?
## concept
**Kernel Inception Distance** - Maximum Mean Discrepancy (see formulas above) between last layers of an inception model

"KID uses the Maximum Mean Discrepancy (MMD), a non-parametric statistical test used to determine if two sets of samples originate from the same distribution." ~ https://apxml.com/courses/synthetic-data-gans-diffusion/chapter-5-evaluating-synthetic-data-quality/distributional-metrics-kid
kernel function - some sort of distance measure, DrumGan uses the one from formulas linked above
## differences with FAD
sounds kind of similar to [[Frechet Audio Distance]] no?
comparing distributions of real and fake data

| aspect              | FAD                                             | KID                           |
| ------------------- | ----------------------------------------------- | ----------------------------- |
| embeddings model    | VGG                                             | Inception                     |
| comparison function | distance between fitted multi-variate gaussians | MMD over all pairs of samples |
|                     |                                                 |                               |

seems like FAD only takes into account the means/variances of the two (multi-variate) distributions, while KID iterates over all sample pairs

unbiased estimator, better for small sample sizes
no gaussian assumption
sensitivity to outliers (if the drumgan kernel - IMQ - is used)
## implementation
ok so what [[DrumGAN]] does is iterate in batches of 5000 over real/fake data until fake data runs out
so potentially not looking at all of the real data?
wtf