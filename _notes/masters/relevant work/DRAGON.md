deez nuts
https://arxiv.org/pdf/2504.15217
Claude summary:
![[Pasted image 20260521204748.png]]
## conceptually
reward optimization for audio diffusion models

currently used metrics for audio generation models are misaligned with human preferences/objectives
- !! very true, looking at you [[Frechet Audio Distance]]

there exist RL-like methods to optimize for a given reward function
- none of them allow instance-to-distribution or distribution-to-distribution rewards (like FAD or Vendi)
- some require reference data specially constructed for the reward
- DRAGON allows cross-modal supervision by learning from distributions rather than exact point-wise matches

optimization itself - split generations into two sets, D+ and D-, push to D+ and away from D- by optimizing the KL divergence
- loss functions - idk, latent diffusion specific stuff, w/e
## misc notes
- uses [[The Vendi Score]] in CLAP embedding space as a measure of diversity, yis
- ". Hence, we conclude that DRAGON can learn from ungrounded text-only music descriptions" the fuck
- some FAD calculation discussion, with two approaches: MA(?) and FADTK in appendix C.2
![[Pasted image 20260521223939.png]]
- convolutional VAE arch similar, nice
![[Pasted image 20260521224037.png]]