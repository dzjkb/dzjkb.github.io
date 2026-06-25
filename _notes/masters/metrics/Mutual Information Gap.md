for a given ground truth feature F:
1. compute mutual information between F and each latent dimension
2. `MIG = <highest> - <second_highest> / entropy(F)`
average across F to get a single scalar

can I calculate this for a PCA-reduced latent space? my first guess would be yes