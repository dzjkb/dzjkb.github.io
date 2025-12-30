multiscale spectral loss

[[Frechet Audio Distance]]
- eh idk, maybe if we choose a legit reference set?
- does this turn to like the diversity thing but opposite?
- basically - how similar is the sample to the reference set?
- but wait I can't use this to evaluate reconstruction since the generated samples will, by definition, be close to the reference (train) set

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