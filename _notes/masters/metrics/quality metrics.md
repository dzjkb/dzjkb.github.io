multiscale spectral loss

[[Frechet Audio Distance]]
- also measures diversity implicitly
- basically fitting a gaussian to target/reference sets and comparing mean/covariance
- meh

[[Kernel Inception Distance]]
- also measures diversity implicitly
- maximum mean discrepancy between point clouds
- better

[[Mean Opinion Score]]
- used by [[MelGAN]]
- and literally everyone else

[mushra](https://en.wikipedia.org/wiki/MUSHRA)
* used by [[SoundStream]]
* kinda like a MOS
* specifically for codecs

[[ViSQOL]]
- like an automatic MOS
- models trained on audio pairs
- codec specific, probably not our usecase

ABX similarity tf?
- like a MOS but similarity to some reference I think?
## notes
difference between FAD and KID:
- FAD is biased, KID is not
- FAD relies on the assumption that points are from a Gaussian distribution, can be fooled by weird shapes
- KID more closely compares the actual shapes of the point clouds
- self note: KID seems to generally be better, but report both for completeness/reference to previous work