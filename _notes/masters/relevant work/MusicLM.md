2023 - Borsos, Engel
continuation of AudioLM
https://google-research.github.io/seanet/musiclm/examples/
https://arxiv.org/pdf/2301.11325

- based on AudioLM 
- compares itself to [Mubert](https://github.com/MubertAI/Mubert-Text-to-Music) and https://github.com/riffusion/riffusion-hobby, but no paper there, w/e, closest thing probs jukebox?
- tokens - whatever, quantized semantic something something
- uses [SoundStream](https://arxiv.org/abs/2107.03312) as the decoder, just a bunch of residual conv1ds
- aaaand for the quantization -> RVQ
- only 24KHz sr
# goals/evaluation
- [x] what are the goals/evaluation methods of MusicLM?

!!!
extends the domain - _we model a large variety of long music sequences beyond piano music (from drum’n’bass over jazz to classical music)._
### experiments
- comparing to baselines (A/B tests)
- ablation - no semantic modeling (FAD, MCC, KLD)
    - _we also confirm this qualitatively by listening to the samples._ lmao that's what I'm saying bro
- all the prompt stuff exploring semantics on the website
    - with the evaluation being just samples shown
    - one of them is fixing=>unfixing acoustic tokens, which results in larger diversity - no quantification of the diversity though
- evaluating training data memorization
### evaluation methods
**[[Frechet Audio Distance]]**
- we meet again

**KL divergence of classifier outputs**
- interesting, a method for comparing with the ground truth on a more abstract level - many distinct audio snippets can be good for a given text description

**MuLan Cycle Consistency** - comparing embeddings, w/e

**A/B preference rating**
- A/B rating task - one text description, two audio samples
- rating from 1 to 5, where 3 is no preference and the other values are strong/weak preferences for either sample A or B
- asked _not_ to take quality into account since it's covered by FAD
- more than two models - to aggregate count the number of "wins"