What does using a RAVE-like VAE give me vs. using a pure GAN approach such as StyleGAN2?

Claude convo about this: https://claude.ai/chat/b64accfd-d614-4c22-ab15-047d665b40f9 (qed acc)
# "theoretical" advantages
1. mode covering rather than seeking behavior
    - not sure this still applies to the more modern variants, but might be worth noting
2. ~~extractable features~~ - doable in GANs now
3. ~~invertible (approximate posterior)~~ - also doable, albeit isn't inherent and requires an additional net
    - GANs require a separate encoder, which is what DrumGAN VST does to be able to work on existing samples
4. ~~doing both feature extraction and 48kHz audio at the same time~~ idk doesn't sound like a real advantage
    - !! DrumGAN VST does 44.1kHz at the cost of a lower audio length though, StyleGAN does 16kHz
5. simplicity
#### sampling rate
can I somehow argument that VAEs have better capacity to represent 48kHz audio in real-time?
# empirical advantages
ideas:
- different (smaller) net sizes? nope, they're comparable
    - shit drumGAN generator is super small, <5M
    - StyleGAN is 25.5M and it doesn't have to change with a higher sample rate, although the quality would probably drop
    - still, comparable
- different compression levels? yep, it's higher, 6x if we account for the SR difference and 2x otherwise (2x larger latent space)
- or maybe just better performance in the meaningful control directions task?
# observed differences dump

|                      | me    | stylegan2                 |
| -------------------- | ----- | ------------------------- |
| latent size          | 256   | 512 latent + 512 "style"  |
| extracted directions | PCA   | early layer factorization |
| compression ratio    | 192:1 | 31:1                      |
model sizes for StyleGAN, drumgan config:

| Network               | Params              |
| --------------------- | ------------------- |
| Generator (G)         | 25.5 M (25,511,443) |
| Discriminator (D)     | 55.8 M (55,848,449) |
| G_ema (EMA copy of G) | 25.5 M (25,511,443) |
| Total unique (G + D)  | ~81.4 M             |
and for my VAE:

| model                                | params |
| ------------------------------------ | ------ |
| no-NF (enc + dec, roughly half each) | 65.2M  |
| NF (enc + dec + NF posterior)        | 63.6M  |
| NF posterior                         | 988K   |
| NF prior                             | 988K   |
# summary paragraph
Why go the VAE route rather than GANs?
- _quality trade-off_: mitigated by 2-stage training
- _mode-covering_: rather than mode-seeking GAN behavior
- _inherent encoder_: rather than requiring a separately trained network
- ~~_inherent unsupervised latent direction discovery_:~~ SeFa does this inherently for GANs too
- _effectiveness_: SOTA papers don