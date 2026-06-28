experiments on the [[VCTK]] dataset
# experiments comparison
## standard
- #1 (logdir `version_1`)
    - maybe the lr is too small?
    - but the discriminator loss is slowly starting to go up instead of down, which is a very good signal
    - I can probably bump up the batch size to 12
    - the discriminator loss went up a bit and started falling again, reconstruction val/train losses are very much plateauing
    - the discriminator vs. model lr might be imbalanced
    - for some reason lr decay took ~670k steps even though it's set to 100k
- #2 (logdir deleted)
    - modified LRs
    - the loss is way too high, wtf, staying at 11 when the previous run dropped to 4 already at that time
    - initial 2e-4 is too high
    - loss staying equal at 11 for 3 epochs straight, stopping
- #3 (logdir `version_2`)
    - same as #2 but with the initial LR back to 1e-4
    - proper learning, but 100k steps for encoder learning seems way to small
    - also the grad norms fall drastically (10-20x) for phase 2, not sure this is ok?
- #4 (logdir `version_3` and continuation in `version_4`)
    - modified reduction to use `Conv1d`, way longer encoding training (1M steps)
        - ok that reduction is pretty much the same thing but w/e
    - seems to be behaving differently (better) than #3 for some reason, nice
    - fixed bug where discriminator losses (feature matching/adversarial) weren't actually propagated to the VAE
    - ok generally good direction - static noise disappeared, feature matching loss started falling
    - but it quickly plateaud (4 epochs, more than enough to conclude it's not going anywhere) at a level which still had artifacts
    - like a blurry phase-mangled type of sound
- #5 (`v4` config, `version_5` logdir)
    - a bit more reduction in the earlier layers, smaller model overall, smaller target LR (maybe that should jump a bit for phase 2?)
    - could probably do even more reduction in the earlier layers
    - pausing at 500k
    - resuming, but shi the models are just probably to small (192:1 compression ratio vs 32:1 for [[RAVE]])
- #6 (`v5` config, `version_6` logdir)
    - even more reduction, length after encoder = 3
    - lower latent/higher reconstruction loss than #5 first 500k steps, maybe this is better for encoder learning?
    - as per claude - aggressive compression at the beginning (stride 8) probably isn't a good idea
- #7 (`v6` config, `version_7` logdir)
    - a too large LR would give noisy losses which would suggest v5 just reached its limit of reconstruction quality?
    - `latent_size: 512` which gives 96:1 compression, `capacity: 80`
    - uniform strides, 1.2x upscaled decoder size
    - fuck the dilations aren't reversed in the decoder
- #8 (`v6` config, `version_8` logdir)
    - same as #7 but with dilations fixed
    - ok ok it's performing worse than **#4** for phase 1, phase 2 was looking good at the start but then began diverging eh
    - maybe bigger latent size doesn't help since the encoder can't use all of the dimensions anyway, and it's more about the encoder capacity?
- #10 (`v7` config, `version_9` `version_10` `version_11` logdirs)
    - 256 latent size, 128/153 capacity, 4-4-4-4 strides, rest same
    - shitty, all losses are going up, why?
    - the audio isn't that bad though, huh
- #11 (`v8` config, `version_12` logdir)
    - going back to the #4 config with higher LRs
    - yeah no
    - hypothesis: disc LR still too high, phase 1 reconstruction too bad and discriminator job is too easy
        - disc lr generally lower than generator lr, RAVE does `1e-4` and `1e-3` so yeah that makes sense
- #12 (`v8` config, `version_13` continued in `version_14` logdir)
    - disc lr to `1e-5`, generator lr to `1e-4` falling to `5e-5`
        - actually decreased disc lr to `5e-6` later
    - way longer latent warmup, bit longer lr decay, 1M phase 1 steps
    - higher latent loss at the start which would make sense, but the reconstruction loss is the same - which could mean worse representation?
        - a bit better reconstruction at the end of phase 1 though, idk, and the latent loss started falling a bit although it didn't go to #11 levels
    - better phase2 behavior - pretty much oscillating at the same levels throughout the whole duration
        - discriminator loss falling at the start and quickly arriving at the plataeu
- #13 (`v9` config, `version_15` logdir)
    - changes: 150k latent warmup steps; adding discriminator scores to fm loss; adv/fm loss warmups over 30k steps; bumped up adv loss `0.5` -> `0.7`; end of phase 1 checkpoint
- #14 (`v9_phase2` config, `version_16` logdir)
    - continuing the same setup as #13, pretty much the same results
    - reconstruction loss is slightly lower than #12; adversarial/discriminator/feature losses are all higher
    - the reconstructions have a slightly more audible high-pitched noise
    - seems that it's the end-of-phase-1 noise that didn't fully subside and that it's correlated to lower reconstruction loss/higher adv losses
    - shit
- #15 (`v9_phase2_v2` config, `version_17` logdir)
    - raised losses: adversarial to `1.5`, feature to `25.0`
    - want to go extreme in the other direction see what happens
    - higher reconstruction loss between #12 and #14 apparently isn't an issue
    - ok reconstruction loss actually didn't go higher than #13?? I guess the loss warmups are very significant here
    - not looking good, feature matching loss still slightly rising, adversarial loss is lower though
- #16 (`v9_phase2_v3` config, `version_18` logdir)
    - same thing but w/o adv loss warmup
    - losses seem to be converging to the same values, just approaching from different directions
    - suggesting the warmup isn't all that necessary, interesting
    - what do you think Claude?
- #16 (`v9_phase2_v3` config, `version_19` logdir)
    - small adv warmup, cut discriminator almost 3x - removed last conv layer in MPD (Claude says it makes sense because we're not reducing coverage, just complexity of the feature maps which might address the problem directly)
    - ok didn't change much lmao bye, last experiment for now
## with normalizing flows
- #1 (`nf_v1` config, `version_0` logdir)
    - 4 layers both prior+posterior, hidden size = latent size, latent warmup 0 epochs, gradient clipping 5.0, other best params from the standard experiments
    - ok at least it's not `nan`ing but it aint learning shit, reconstruction loss barely moving over 11
- #2 (`nf_v1` config, `version_1` logdir)
    - same but w/o gradient clipping, I need em signals
    - decrease capacity `96->94` to preserve model size compared to non-NF model (note: we're counting the prior too which doesn't take part in reconstruction, so effectively the model is smaller)
    - same badly conditioned training as before - plateu at ~11.20
    - def mode collapse
- #3 (`nf_v1` config, `version_2` logdir)
    - decreasing latent weight `1e-4 -> 2e-5` (`1e-5` gave `nan`s)
    - mode collapse again, come on
- #4 (`nf_v1` config, `version_3` logdir)
    - switch scale network back to `tanh`
    - still mode collapsing, ahhhhhhhhh
- #5 (`nf_v1` config, `version_4` logdir)
    - oh my god is it the missing minimal variance constant `std = softplus(scale) + 1e-4`
    - training on a random half of the train set for speedup
    - nope still collapsing
    - the latent loss has a nice curve though, that's a clue, try less weight - `5e-6`
- #6 (`nf_v1` config, `version_5` logdir)
    - switched to zuko implmenetation, `256->64->64->256` MLPs in the coupling transforms instead of `256->256->256`, which gives ~250K features (5x less than the previous iterations); switched to `1e-4` latent loss weight
    - ok it actually works, it's starting to learn proper reconstructions
    - the reconstruction loss curve is slightly slower at the beginning than no-NF `version_13`, not sure if it's (a) the halved train set or (b) the slightly lower capacity or (c) the flows
    - maybe I should preserve the capacity at first to verify that flows improve the reconstruction? at very little parameter cost
- #7 (`nf_v1` config, `version_6` logdir)
    - switched MLPs to `256->256->256` size
    - same exact reconstruction loss curve, higher latent loss (but still with a similar curve)
- #8 (`nf_v1` config, `version_7` logdir)
    - 8 transform layers instead of 4
    - ok better reconstruction loss curve, very similar to no-NF `version_13`
    - lower latent loss (not sure how comparable the abs values are)
    - but a result like same reconstructions + better generations is nice
- #9 (`nf_v1` config, `version_8` logdir)
    - 10 nf layers, full dataset, full discriminator
    - ok let it ride
    - aight seems good, pretty similar in sound to no-NF versions, metrics seem better though
- #10 (`nf_v2` config, `version_9` logdir)
    - planned
    - mel scale; `stft_eps = 1e-4`
## prior
- #0 (`v1.yaml`, `version_0` logdir)
    - ok first tries, this goes quick af; no problem fitting the prior, the random test/val split seems useless but w/e
# latent space analysis
# eval metrics

no-NF: `version_14`, NF: `version_8`, reference set: `vctk_words_v2/audio_val`
CLAP embeddings using `music_audioset_epoch_15_esc_90.14.pt`

| model         | mode           | KID    | FAD    | Vendi  |
| ------------- | -------------- | ------ | ------ | ------ |
| no-NF         | reconstruction | 0.0013 | 0.0203 | 5.2167 |
| NF            | reconstruction | 0.0014 | 0.0210 | 5.4041 |
| RAVE          | reconstruction | 0.0014 | 0.0205 | 5.3838 |
| no-NF         | unconditional  | 0.0765 | 0.7830 | 3.9264 |
| NF            | unconditional  | 0.0772 | 0.7977 | 3.7587 |
| RAVE+prior20M | unconditional  | 0.0737 | 0.7343 | 3.8908 |
| RAVE+prior3M  | unconditional  | 0.0670 | 0.6852 | 4.0574 |
| NF+prior      | unconditional  | 0.0138 | 0.1673 | 6.2118 |
| **self**      | **comparison** | 0.0000 | 0.0000 | 5.0779 |

same but CLAP using `music_speech_epoch_15_esc_89.25.pt` and only the 3M rave prior:

| model      | mode           | KID    | FAD     | Vendi  |
| ---------- | -------------- | ------ | ------- | ------ |
| no-NF      | reconstruction | 0.0015 | 0.0248  | 5.9449 |
| NF         | reconstruction | 0.0015 | 0.0247  | 6.1620 |
| RAVE       | reconstruction | 0.0017 | 0.0268  | 5.9428 |
| no-NF      | unconditional  | 0.0534 | 0.5886  | 3.9058 |
| NF         | unconditional  | 0.0549 | 0.6049  | 3.5694 |
| RAVE+prior | unconditional  | 0.0535 | 0.5804  | 4.3253 |
| NF+prior   | unconditional  | 0.0115 | 0.1491  | 5.6436 |
| **self**   | **comparison** | 0.0000 | -0.0001 | 6.1212 |

parameter counts:

| model                         | params |
| ----------------------------- | ------ |
| no-NF (enc + dec)             | 65.2M  |
| NF (enc + dec + NF posterior) | 63.6M  |
| RAVE (enc + dec)              | 31.5M  |
| RAVE (prior20M)               | 19.8M  |
| RAVE (prior3M)                | 3.0M   |
| NF prior                      | 988K   |

| model    | gilbo                         |
| -------- | ----------------------------- |
| no-NF    | 115.9395 nats (167.2653 bits) |
| NF       | 130.1579 nats (187.7782 bits) |
| NF+prior | 107.5000 nats (155.0897 bits) |
