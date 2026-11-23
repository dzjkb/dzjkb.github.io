# Controllable Neural Drum / Instrument Synthesis, 2020–2026
## A related-work map for unsupervised, post-hoc latent control

This map organises the field into three threads and places the thesis within them. The organising axis is the **control mechanism** — *how* a user steers generation — because that, not the generative family per se, is where the approaches genuinely differ.

### A vocabulary for "control mechanism"

- **Supervised conditioning** — labelled attributes (perceptual descriptors, class) fed at training time; the model learns `p(x | c)`. Interpretable by construction, but requires labels.
- **Learned disentanglement** — architectural or objective constraints (e.g. auxiliary classifiers, adversarial criteria) that shape the latent during training so that axes carry distinct factors. Partly to fully supervised.
- **Post-hoc unsupervised discovery** — directions found *after* training by analysing an already-learned space (PCA on encoded data or on activations; closed-form weight factorisation). No labels, no retraining. *This is the thesis's mechanism.*
- **Text / symbolic conditioning** — natural-language prompts or MIDI drive generation; control lives in the conditioning model, not in a navigable latent geometry.

---

## Thread 1 — The GAN-control thread

Adversarial synthesis with control layered on the generator or its latent space. This is the lineage the thesis benchmarks against.

| Work (year, venue)                                                                                                         | Model                                                                         | Control mechanism                                                                                                                                                 | Relevance to thesis                                                                                                                                                                      |
| -------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Donahue et al. — WaveGAN / *Adversarial Audio Synthesis* (2019, ICLR; arXiv 1802.04208)                                    | Raw-waveform GAN                                                              | None beyond `z` sampling                                                                                                                                          | Ancestor; establishes audio GAN synthesis                                                                                                                                                |
| Nistal, Lattner, Richard — **DrumGAN** (2020, ISMIR; arXiv 2008.12073)                                                     | Progressive-Growing GAN on complex STFT (real+imag), 16 kHz / 1 s             | **Supervised** perceptual-feature conditioning (AudioCommons: brightness, hardness, depth, roughness, boominess, warmth, sharpness)                               | Canonical "control = supervised conditioning" contrast; eval via IS/FAD/KID + feature-coherence                                                                                          |
| Drysdale, Tomczak, Hockman — **ASDS** *Adversarial Synthesis of Drum Sounds* (2020, DAFx)                                  | Conditional Wasserstein GAN, 44.1 kHz waveform                                | **Supervised** drum-class conditioning (3-dim class embedding)                                                                                                    | Class-level control only                                                                                                                                                                 |
| Drysdale, Tomczak, Hockman — **Style-based drum synthesis with GAN inversion** (2021, ISMIR LBD)                           | StyleGAN-style waveform synth (Z→W)                                           | **Post-hoc / emergent unsupervised** feature separation (pitch, envelope, loudness), layer-wise; + GAN inversion (encoder) for real sounds                        | Closest ancestor of the thesis's post-hoc idea; public code (`style-drumsynth`)                                                                                                          |
| Lavault et al. — **StyleWaveGAN** (2022; arXiv 2204.00907)                                                                 | Style-based GAN, 44.1 kHz                                                     | **Supervised** conditioning on drum type + perceptual features                                                                                                    | Style architecture but supervised control                                                                                                                                                |
| Nistal, Aouameur, Velarde, Lattner — **DrumGAN VST** (2022, ICML workshop; arXiv 2206.14723)                               | DrumGAN + encoder + continuous class controls                                 | **Supervised** class + **encoder inversion**; random-`z` "variation" is uninterpretable by design                                                                 | Shows a GAN needs a *bolted-on* encoder for analysis; random-direction variation ≠ interpretable control                                                                                 |
| **Ramires, Juras, Parker, Serra — *A Study of Control Methods for Percussive Sound Synthesis Based on GANs*** (2022, DAFx) | StyleGAN2 on 1024×32 complex spectrograms, 16 kHz / 1 s, latent = style = 512 | Compares raw-**Z** vs **SeFa** (post-hoc unsupervised, closed-form W-space factorisation) vs **InterFaceGAN** (supervised SVM hyperplanes on AudioCommons labels) | **Primary baseline.** Eval = Creativity Support Index user study (n = 14); FAD 2.689; SeFa preferred but *not* statistically significant; no objective direction-interpretability metric |
| Drysdale & Hockman — **Latent Space Exploration for Drum Samples** (2024, CBMI; IEEE)                                      | Extends their GAN                                                             | Latent arithmetic, dimensionality reduction, visualisation; low `dz` (e.g. 3) for navigability                                                                    | Most recent direct descendant of the Ramires premise; navigation-focused                                                                                                                 |

**Method lineage (computer vision, context).** The discovery methods used above originate in image GANs: **InterFaceGAN** (Shen et al., 2020, CVPR — supervised SVM directions);
**GANSpace** (Härkönen et al., 2020, NeurIPS; arXiv 2004.02546 — post-hoc unsupervised PCA on activations/W, encoder-free); **SeFa** (Shen & Zhou, 2021, CVPR; arXiv 2007.06600 — post-hoc
unsupervised closed-form weight factorisation, encoder-free); later **ReSeFa** (2022) and **DragGANSpace** (2025; arXiv 2509.22169). Only InterFaceGAN/GANSpace/SeFa crossed into drums (via Ramires); the newer vision methods have not been ported to audio. 
**Key point for the thesis:** these establish that post-hoc interpretable directions in GANs do *not* require an encoder — so "you need an encoder for PCA" is not a valid VAE advantage.

---

## Thread 2 — The VAE-latent-navigation thread

The latent space *itself* is the control surface: encode/sample, then move within a *prior*-regularised, often low-dimensional latent. This is the thesis's family.

| Work (year, venue)                                                                                                                                      | Model                                                                                                                               | Control mechanism                                                | Relevance to thesis                                                                                                                                    |
| ------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Aouameur, Esling, Hadjeres — **Neural Drum Machine** (2019)                                                                                             | Conditional Wasserstein Autoencoder on magnitude spectrogram + multi-head CNN                                                       | **Post-hoc unsupervised**: PCA on 64-dim latent → 3 control dims | Early PCA-on-latent-for-control; authors note dims were *not* shown to be perceptually meaningful                                                      |
| Esling et al. — **Flow Synthesizer** (2019–20)                                                                                                          | VAE + normalizing flows for synthesizer control                                                                                     | Learned macro-controls / partial disentanglement                 | VAE+flow route to controllable dims                                                                                                                    |
| Esling group — **Timbre latent space: exploration and creative aspects** (~2021)                                                                        | VAE with perceptual regularisation                                                                                                  | Hybrid: perceptual alignment + unsupervised remainder            | Aligns latent axes to established timbre spaces                                                                                                        |
| **Caillon & Esling — RAVE** (2021; arXiv 2111.05011)                                                                                                    | VAE, two-stage training (representation learning + **adversarial fine-tuning**), ~23 Hz latent time-series, real-time               | Post-training PCA on latent for compact control                  | **Architectural starting point.** Note: RAVE is itself a VAE-**GAN** hybrid                                                                            |
| Limberg et al. — **GESAM** *Generative Sample Map* (2024, ACM ICMR)                                                                                     | VAE learns descriptive 2D map → conditions a Transformer                                                                            | Navigate a 2D map (unsupervised map)                             | Modern "VAE latent as UI"; transformer generator                                                                                                       |
| Limberg, Schulz, Zhang, Weinzierl — **Pitch-Conditioned Instrument Synthesis from an Interactive Timbre Latent Space** (2025, DAFx25; arXiv 2510.04339) | VAE 2D timbre latent with **explicit disentanglement constraints** (instrument/family classifiers) + Transformer, pitch-conditioned | **Learned disentanglement** (semi-supervised)                    | **Most current comparator** for "VAE latent for interpretable control"; contrast: they *learn* disentanglement, thesis discovers directions *post-hoc* |
| Simi-SFX (2024; arXiv 2412.18710)                                                                                                                       | Similarity-based conditioning for SFX                                                                                               | Similarity conditioning                                          | Restates the open problem: unclear which feature each sub-latent controls                                                                              |

---

## Thread 3 — The latent-diffusion / neural-codec thread

Where the field's momentum went after ~2023: a (usually VAE-defined) latent provides a compact, well-conditioned space, and a diffusion or transformer prior generates *within* it. Control typically comes from text/symbolic conditioning rather than latent geometry.

| Work (year, venue) | Model | Control mechanism | Relevance to thesis |
|---|---|---|---|
| Kumar et al. — **DAC** *Descript Audio Codec* (2023) | RVQ-GAN neural codec (VQ-GAN) | None (compression) | The compression substrate reused as a diffusion latent; a VQ-**GAN**, not a VAE |
| Rouard & Hadjeres — **CRASH** (2021, ISMIR) | Score-based / diffusion drum synth | Class interpolation on the noise latent | Early diffusion drums |
| Evans et al. — **Stable Audio** *Fast Timing-Conditioned Latent Audio Diffusion* (2024, ICML; arXiv 2402.04825) | Latent diffusion; latent = VAE-GAN hybrid (recon + adversarial + KL ≈ 1e-4) | **Text + timing** conditioning; latent is *not* the control surface | Shows why the field standardised on VAE-defined latents (continuous, ~Gaussian, encoder-accessible) — but deliberately weak KL, so *not* structured for control |
| Evans, Parker, Carr, Zukowski, Taylor, Pons — **Stable Audio Open** (2024; arXiv 2407.14358) | Open latent-diffusion; VAE latent 64-dim @ 21 Hz | Text | Primary-source autoencoder details |
| Demerlé, Esling, Doras, Genova — **Combining audio control and style transfer using latent diffusion** (2024; arXiv 2408.00196) | Diffusion autoencoder + **adversarial disentanglement** of local (structure) vs global (timbre) | Learned disentanglement + reference-based transfer | The RAVE lab's *own* move beyond pure VAE for control — cite and distinguish |
| Mancusi et al. — **Latent diffusion bridges for unsupervised timbre transfer** (2025; arXiv 2409.06096) | Latent diffusion bridges | Unsupervised transfer | Recent timbre-control point |
| **Break-the-Beat!** — Controllable MIDI-to-Drum Audio Synthesis (2025–26) | Fine-tuned text-to-audio LDM + content encoder | **Symbolic (MIDI)** + reference-timbre | Modern drum-specific control, symbolic route |
| **Sec2Drum-DAC** — PCA-DAC Latent Diffusion (2026) | Latent diffusion predicting **72 PCA coords of frozen DAC latents** | Symbolic conditioning | A modern echo of "PCA on the latent" — but for a compact *diffusion target*, not for control |

---

## Where this thesis sits

**Architecture.** Inherits RAVE's two-stage VAE-GAN recipe (reconstruction + adversarial fine-tuning with an RVQGAN discriminator), but replaces RAVE's ~23 Hz latent *time-series* with a
**single global latent vector** over ~1 s of audio. The adversarial component serves reconstruction fidelity; the VAE/KL component shapes the latent geometry.

**Goal.** `z → x` generation with **post-hoc, unsupervised, interpretable control**: directions found by PCA on the aggregate posterior of encoded training data, then applied to sampled `z`. Real-sound *inversion* is out of scope.

**Position between the threads.**
- *vs Thread 1 (GAN-control):* Shares the post-hoc unsupervised discovery mechanism with Ramires/SeFa, but discovers directions in a **VAE latent that is itself the generation space**. A GAN's input `z` is isotropic by construction, so its structure lives downstream in activation/style space (hence GANSpace/SeFa on `W`); the VAE's aggregate posterior is non-isotropic and lives where you sample and decode, so data-PCA directions apply directly — no encoder-free indirection, no style-space detour.
- *vs Thread 2 (VAE-navigation):* Shares the VAE-latent-as-control-surface premise with Limberg et al., but relies on **post-hoc discovery rather than learned/semi-supervised disentanglement**, and on a **single global vector rather than a 2D map or per-frame sequence**.
- *vs Thread 3 (latent-diffusion):* Diverges deliberately. Those systems use the VAE latent for *compression* (weak KL) and put control in a text/symbolic prior; this thesis leans *on* the latent for control, accepting a stronger structure/fidelity trade-off.

**Contribution.** A rigorous, seed-controlled, **objective + subjective comparison of direction interpretability** across a VAE and a GAN (SeFa) on **matched drum data** — the measurement the prior drum literature has not made (DrumGAN measured coherence only for its supervised conditioning; Ramires ran an underpowered CSI study with no objective direction metric).

**Caveats to state up front.**
- *Locatello et al. (2019, ICML; arXiv 1811.12359):* unsupervised disentanglement is impossible without inductive biases and is highly seed-dependent → claim *usable* post-hoc directions, not guaranteed disentangled factors, and report across multiple seeds.
- Perceptual descriptors (AudioCommons timbral models) are correlated, so disentanglement metrics (MIG/DCI/SAP) computed against them are approximate proxies.
- The single-vector bottleneck biases toward global/timbral structure and trades reconstruction fidelity for compact, navigable control — well matched to one-shots / drum hits, lossier for arbitrary polyphonic slices.

---

*Notes: dates given as (year, venue); arXiv IDs included where a preprint is the practical source. Entries marked 2025–26 are recent and worth verifying venue/version before final citation. Bold rows are the anchor works for the thesis's argument.*
