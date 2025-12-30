**Virtual Speech Quality Objective Listener** - a metric for percieved audio quality
```
It uses a spectro-temporal measure of similarity between a reference and a test speech signal to produce a MOS-LQO (Mean Opinion Score - Listening Quality Objective) score.
```

implementation from google [available on github](https://github.com/google/visqol), with a simple python api, lets go
- also a [descript fork](https://github.com/descriptinc/visqol)
[ViSQOL v3: An Open Source Production Ready Objective Speech and Audio Metric (arxiv)](https://arxiv.org/abs/2004.09584)

basically an automatic MOS estimate

![[Pasted image 20250907205714.png]]

not sure it makes sense, training a custom model for the NSIM -> MOS mapping would be too much

_In practice, users try it for other use cases, such as denoising, regression testing on preprocessing, and deep learning-based generative models. ViSQOL performs reasonably for some of these, and poorly for others._

used in
- [[RVQGAN]]
- [[SoundStream]]
### Qs
- [x] what papers use ViSQOL?
- would ViSQOL require training a custom model?