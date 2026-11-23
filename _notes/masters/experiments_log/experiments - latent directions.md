- Directions per run = top-K by that run's own ordering.
- 'per sigma' = one standard deviation of the latent distribution the run uses along the direction (N(0, I) prior: 1 latent unit; encoded-clip PCA: sqrt of the PC variance).
- Effects are in real-validation-data std units.

| | drumgan_s6b8_i149850 | vae_nfv3_flowprior_sefa | vae_nfv3_flowprior_encpca | vae_nfv3_N01_sefa |
|---|---|---|---|---|
| top-1 share | 0.2176 | 0.0673 | 0.0661 | 0.021 |
| top-10 share | 0.4127 | 0.4339 | 0.4161 | 0.1297 |
| eff. rank / dim | 61 / 128 | 80 / 256 | 83 / 256 | 212 / 256 |
| dirs w/ control | 3 / 10 | 10 / 10 | 10 / 10 | 10 / 10 |
| median top |rho| | 0.59 | 1.0 | 0.97 | 0.92 |
| median top consistency | 80% | 100% | 100% | 95% |
| median top |effect|/sigma | 0.09 | 0.11 | 0.09 | 0.1 |
| distinct axes | 3 (dirs 0, 2, 6) | 9 (dirs 0, 1, 2, 3, 4, 5, 7, 8, 9) | 7 (dirs 0, 1, 2, 3, 5, 6, 9) | 6 (dirs 0, 1, 2, 3, 6, 7) |
| median profile overlap | 0.73 | 0.67 | 0.67 | 0.84 |
| sigma step (latent units) | 1.00-1.00 | 1.82-2.50 | 3.08-4.92 | 1.00-1.00 |

## Subspace overlap (same latent space)

$||Q_a[:, :k]^T Q_b[:, :k]||_F^2 / k$ on orthonormalised edit directions (1 = same subspace; random = k/d).

| runs                                                 | k=1   | k=5   | k=10  | random (k=10) |
| ---------------------------------------------------- | ----- | ----- | ----- | ------------- |
| vae_nfv3_flowprior_sefa vs vae_nfv3_flowprior_encpca | 0.013 | 0.756 | 0.812 | 0.039         |
| vae_nfv3_flowprior_sefa vs vae_nfv3_N01_sefa         | 0.067 | 0.365 | 0.549 | 0.039         |
| vae_nfv3_flowprior_encpca vs vae_nfv3_N01_sefa       | 0.005 | 0.235 | 0.536 | 0.039         |

