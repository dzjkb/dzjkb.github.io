reference - https://apxml.com/courses/vae-representation-learning/chapter-5-disentangled-representation-learning-vaes/disentanglement-metrics?__cf_chl_f_tk=0h3WRd_6XAtTyDqauceGmplBFPYVeWw_n3KYFi6V6LM-1783100440-1.0.1.1-zP0OdaqMC_jspFE.8Z6EDOhrOboovQ6.TrysMc1.ksI

for a given ground truth feature F:
1. compute mutual information between F and each latent dimension
2. `MIG = <highest> - <second_highest> / entropy(F)`
average across F to get a single scalar

can I calculate this for a PCA-reduced latent space? my first guess would be yes

this tries to answer "are any latent dimensions highly correlated for this feature", but I already get that from the linear classification