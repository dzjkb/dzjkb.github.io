[overleaf project](https://www.overleaf.com/project/6199162e6c5c06a7329b20d7) - moved to local though
conceptual framework is in [[thesis goals]]
each section has references
## Wprowadzenie
[[thesis - 1. intro]]
### synteza w muzyce
inspo:
- that guys PhD thesis
- [fadtk paper](https://arxiv.org/pdf/2311.01616)
- [[DrumGAN]]
- [[Neural Drum Machine]]
- [[Crash]]
- [[StyleGANv2 on drums]]
- [[DRAGON]]
    - deez nuts across your face
### modele generatywne a synteza
refs:
- DrumGAN
- Crash
- StyleGAN on perc sounds
### najważniejsze wyniki
- porównywalna/lepsza jakość z single-latent VAE + NF
- meaningful directions in the latent space
## Powiązane prace
[[RAVE]]
[[DrumGAN]]
[[StyleGANv2 on drums]]
[[Crash]] maybe

important - flex about field knowledge here
## Podstawy i narzędzia
### cyfrowe reprezentacja dźwięku
refs:
### modele generatywne - VAE, GAN
refs:
### (: normalizing flows
refs:
### (: PQMF
refs:
## Rozwiązanie
### warstwy konwolucyjne oraz rezydualne
### NF dla rozkładów prior/posterior
### inne architekturalne szczegóły
### (: 2-fazowy trening - discriminator
## Metody Ewaluacji
### (: embeddingi Laion CLAP
[[LAION-audio-630K]]
### (: FAD/KID - metryki referencyjne
[[Frechet Audio Distance]]
[[Kernel Inception Distance]]
### (: Vendi - obiektywna metryka różnorodności
[[The Vendi Score]]
### (: metryki jakości przestrzeni ukrytej
idk wsm skoro ich nie używam?
## Zestawy danych
### (: VCTK words
refs:
- [[VCTK]]
- Qwen forced aligner
### (: Percussive
refs: the internet
### (: SFX
refs:
- [[LAION-audio-630K]]
## Eksperymenty
### hiperparametry, trening
### wyniki - rekonstrukcja, generacja, porównanie z RAVE
tabelki
### wyniki - struktura przestrzeni ukrytej
analiza latent space'u - metryki, wykresy
odczytywanie cech
transfer rmod -> percussive
### (: przykłady generacji
### porównanie ze StyleGAN
## Podsumowanie i wnioski
### wnioski, co się udało osiągnąć
nie musi to być propaganda sukcesu - samo to że zaproponowałem jakieś inne rozwiązanie i je zweryfikowałem jest g
### dalsze kierunki
### wtyczka VST
# references, collected
# graphs
# misc
look at these  for inspo:
- [fadtk paper](https://arxiv.org/pdf/2311.01616)
- [[DrumGAN]]
- [[Neural Drum Machine]]
- (?) [[DRAGON]]
    - deez nuts across your face
# roughly
1. intro
    1. (digital) synthesis in music
    2. new ways of synthesis - high-level concepts with clear semantics
    3. unsupervised extraction of meaningful latent space directions
    4. VAEs are applicable to this task with unconditional generation
2. background
    1. deep learning
    2. generative models - VAEs, GANs maybe?
    3. normalizing flows - briefly
    4. PQMF - briefly
3. my solution
    1. architecture - convolutions, residuals, time domain, pqmf, noise synth
    2. 2-phase training
    3. VAEs vs GANs discussion
4. datasets
    1. processing VCTK to extract words
    2. gathering drum samples
    3. gathering and filtering sfx samples
5. metrics
    1. FAD - meaning, limitations
    2. KID - complementing FAD
    3. Vendi - a modern metric purely for diversity
    4. any latent space metrics?
6. results
    1. quality doesn't decrease compared to other methods (RAVE)
    2. normalizing flow posteriors don't help
    3. normalizing flow priors are key for good uncond. generation
    4. meaningful directions can be recovered
    5. very nice to have - comparison with StyleGAN