The Vendi Score - A Diversity Evaluation Metric for Machine Learning

yisssss
https://arxiv.org/abs/2210.02410
"the Vendi Score, which connects and extends ideas from ecology and quantum statistical mechanics to ML" bruh
[official implementation](https://github.com/vertaix/Vendi-Score)

continuation: [Cousins Of The Vendi Score: A Family Of Similarity-Based Diversity Metrics For Science And Machine Learning](https://arxiv.org/abs/2310.12952)

## conceptually
background:
- diversity based on reference sets - weak because we want diversity to be an intrinsic property
- diversity based on pre-trained classifiers - require high quality labels/classifiers
- other - counting modes; distributions of discrete features
- some domain-specific or model-specific metrics ([gilbo](https://arxiv.org/abs/1802.04874))
- average pairwise (dis)similarity - closely related - in biology known as IntDiv

the Vendi score is inspired by biological metrics of diversity - entropy of the distribution of species

better mathematical properties than average similarity:
- the Vendi score increases linearly with the count of modes in the ds, but intdiv converges to 1 => is less sensitive with more modes
- combining different similarity functions can increase the Vendi score
- the Vendi score takes feature correlation - as in correlation of different combined similarity metrics - into account, meaning it can be even greater if the two metrics describe distinct dimensions of variation

"effective" measure (effective number of dissimilar elements in a sample)
- in the maximum diversity case - `k(x, x') = 0` for all `x, x'` - `VS(X)` reaches the maximum value `size(X) = N`, meaning there's effectively `N` different samples
- in the minimum case - `k(x, x') = 1` for all `x, x'` - `VS(X)` reaches the minimum value 1, meaning there's effectively 1 unique sample
- linear correlation to the number of modes; gives a "base" for comparing values in percentages
- insensitive to the addition of duplicated samples or sample ordering
## mathematically
needs a similarity function:
```
0 <= k(x, x') <= 1

k(x, x) = 1 => identical samples
k(x, x') = 0 => x and x' are the most dissimilar
```

similarity matrix K/n -> eigenvalues -> exponent of shannon entropy
eigenvalues sum to 1 because each element on the diagonal is 1/n
- idk what's the proof but sure
## evals
synthetic 1-D datasets varying in number of modes, variance within modes or mixture ratios - Vendi is better conditioned than avg distance and actually sensitive to the variance

GAN mode collapse - way more fine-grained than number of modes

image generation diversity
refers to "standard" metrics in this case: Inception Score, FID, Precision&Recall (from [[Improved Precision and Recall Metric for Assessing Generative Models]] probs)
![[Pasted image 20260519180656.png]]
```
In these cases, vs can be interpreted as complementing the existing metrics. For example, on lsun Cat, the ADM model achieves a precision score of 0.63 and recall of 0.52, implying that 63% of generated images look like reference images, and that the generated images cover 52% of the reference distribution; however, the low vs suggests that the remaining images have low internal diversity—for example, the model may generate many near-duplicates.
```
- recall understanding - diversity <=> coverage of the reference set
main point - vs agrees with existing metrics + extends them

measuring ground-truth dataset diversity itself
on the example of cifar-100 - diversity within different classes
either in
- the pixel space - visual, diversity is high for classes like fish, low for e.g. cockroaches
- Inception embedding space - more semantic, higher for classes which are hard to classify
## limitations
purely a diversity metric, needs to be paired with a quality metric
- high VS can be achieved by sampling random noise
the similarity function must be carefully selected
- a function that's too sensitive can lead to artificially high scores
- biases in the function lead to biased scores
- influences the interpretation of VS
## embedding distance special case
```
However, when embeddings of the observations (or feature vectors) are available, which is the case in many ml settings and in many of the applications we consider in this paper, one can use similarity functions defined as inner products between the embeddings φ(x) ∈ Rd , with d ≪ n. That is, we can use the similarity matrix K = X⊤X, where X ∈ Rn×d is the embedding/feature matrix with row Xi,: = φ(xi ). The eigenvalues of K /n are the same as the eigenvalues of the covariance matrix X X⊤/n, therefore we can calculate the Vendi Score exactly in a time of O(d2n + d3) = O(d2n). This is the same complexity as existing metrics such as fid (Heusel et al., 2017), which require calculating the covariance matrix of Inception embeddings.
```
implemented as `vendi.score_dual(X)`
this is equal to the vendi score with
```
def k(a, b):
    return np.dot(a, b).item()

vendi.score(normalize(embeddings, norm="l2", axis=1), k)
```

this normalizes all embedding vectors to unit vectors first - doesn't this change the generation landscape?
~~seems suspicious~~
it does, but in very high dimensional spaces angular distance is more meaningful - e.g. CLAP is trained on cosine similarity
and absolute distances get very large very quick