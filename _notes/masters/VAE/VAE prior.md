two options:
1. single latent vector, like in [[NSynth]] - prior is just the enforced distribution
2. a sequence of latent vectors from enforced distribution, like in [[RAVE]] + prior RNN model

sequence way more expressive, wouldn't make sense to combine with normalizing flows probs?

single vector approach enabled because of limited sample length
- allows normalizing flow exploration
- doesn't require prior RNN model