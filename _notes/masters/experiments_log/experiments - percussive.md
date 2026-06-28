# percussive2
- #0 (`v1` config, `version_0` logdir)
    - the mb loss is decreasing while recon loss is 2x higher and oscillating, that's interesting, looks like mode collapse tho fml
    - ok not mode collapse actually, it's learning something, started NaNing though shit
- #1 (`v1` config, `version_1` logdir)
    - switched lr schedule to `1e-5` -> `5e-6`
    - ok good curves, starting very high though but maybe w/e
    - sudden jump in fullband reconstruction loss at ~40k steps, looks like the jumps at the start of phase 2 (but it's not phase 2 yet)
    - wtf
    - reconstructions aren't mode collapsed but sound shite
    - not really improving in the second half
- #2 (`v1` config, `version_2` logdir)
    - raised stft eps to 1e-3; using mel scale for stft loss
    - ok works lessssssssgo, proper curve, recon loss is already at 2.3
    - reconstructions are learning something but there's definitely large emphasis on the low end (the kicks sound nice); the high has a lot of digital static
    - what parameter would I tweak to control this trade-off? seems like something heavily data-dependent 
        - `n_mels` - more would give more resolution to higher frequencies, hm
        - `f_max` - ?
        - mixing mel and linear scales - interesting
    - ok it did start to learn high frequencies too; bumped fm to `50.0` in the middle, it's learning something but very slowly
- #3 (`v1_phase2_vol2` config, `version_4` logdir - note v3 is missing)
    - continuing from #2 end of phase 1; fm `50.0`, constant lr at `1e-4`
    - ok ok ok
    - the losses are kind of rising, similar to the behavior of the VCTK RAVE run
    - but the sound is good
    - kicks, snares, the deeper frequencies sound really nice
    - the weird shimmering noises sound audibly degraded
    - but overall seems like a legit run
    - uncond generations aren't great, sort of like averaged/degenerated sounds, lots of silence or quiet noises
    - if there's something more distinct it's a delicate snare-like sound
    - lots of room for improvement
## NF
- #0 (`nf_v1` config, `version_0` logdir)
    - no latent loss warmup; 10 posterior layers; `1e-5` lr; everything else stays the same
        - `1e-4` latent weight mode collapses, try `1e-5`
    - still mode collapsing, and the latent loss isn't even that low, what?
- #1 (`nf_v1` config, `version_1` logdir)
    - changed latent weight to a constant `1e-6` (the initial value for the warmup)
    - actually started to learn something, the latent loss, naturally, is just soaring
    - but the reconstruction loss is apparently hitting a plateau earlier
    - super weird behavior, both the collapse and the worse learning now
    - maybe 10 is too many nf layers?
    - latent loss just rising indefinitely, not a good sign
- #2 (`nf_v1` config, `version_2` logdir)
    - back to 300k latent warmup length `1e-6` -> `1e-4`; 6 nf layers
    - practically no jump in recon loss for phase2, huh
    - reconstructions are even more shite
    - ok maybe it's not _that_ much of a difference compared to no-NF, but still, NF seems to be degrading rather than improving this
- #3 (`nf_v1` config, `version_3` logdir)
    - 200k latent warmup steps `1e-6` -> `2e-4`; fm loss `40.0`
    - ok idk seems reasonable, proceed with evals
# eval metrics
#### eval1

| model    | mode           | KID    | FAD    | Vendi   |
| -------- | -------------- | ------ | ------ | ------- |
| **self** | **comparison** | 0.0000 | 0.0000 | 21.6521 |
| no-NF    | reconstruction | 0.0045 | 0.1569 | 15.2375 |
| NF       | reconstruction | 0.0040 | 0.1271 | 15.2508 |
| RAVE     |                |        |        |         |
| no-NF    | unconditional  | 0.0537 | 0.8103 | 10.0454 |
| NF       | unconditional  | 0.0504 | 0.7732 | 10.2943 |
| RAVE     |                |        |        |         |
| NF+prior | unconditional  | 0.0180 | 0.3539 | 9.7582  |
