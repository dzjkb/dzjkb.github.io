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