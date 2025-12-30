2021 - google
[Google Blog article](https://research.google/blog/soundstream-an-end-to-end-neural-audio-codec/)
[paper (arxiv)](https://arxiv.org/pdf/2107.03312)

## architecture
also uses a discriminator huhuh

**Encoder**

![[Pasted image 20250329123038.png]]
![[Pasted image 20250329132728.png]]

**Decoder**

![[Pasted image 20250329134241.png]]

**Other details**

- no normalization
- ELU activation
- causal convolutions
- reconstrution + discriminator loss

- `C=16` or `C=32` mostly
    - 2-4x less than [[RAVE]]
# goals/evaluation
200 samples subjectively evaluated by human raters
inspired by [mushra](https://en.wikipedia.org/wiki/MUSHRA) - a method specific for codecs

for development purposes - the quantitative [[ViSQOL]] metric (related to PESQ and POLQA but without licensing restrictions)

- [x] what are the goals/evaluation methods for SoundStream?

**achieved goals**
- reconstruction quality of **continuous audio streams**
- low bitrate of the latent space
- general-purpose, not tailored to only e.g. speech
side "achievements"
- variable bitrate (using dropout on the quantizer layers?)
- realtime streamable inference on smartphone CPUs
- noise supression for speech with no additional latency

**metrics**
- [[ViSQOL]] vs bitrate
- mushra score vs birate
- mushra disaggregated by content type (at specific bitrates)

different disaggs - ViSQOL vs model size, latentcy (total strides), number of quantizers

compared with Opus, EVS, Lyra