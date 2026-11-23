# training procedure
parameters per dataset
two-phase
Adam
feature matching loss
on a GeForce 1080 Ti

what were the three final (no-NF) configs?
- vctk - `logs/vctk_words/version_14/`
- perc2 - `/media/Data/jp_dir/sounds/aws_training/percussive2/version_4/`
- sfx - `logs/sfx1f_short/version_0/`
and NF configs
- vctk - `logs/vctk_words_nf/version_8/config.yaml`
- perc2 - `/media/Data/jp_dir/sounds/aws_training/percussive2_nf/version_3/`
- sfx - `logs/sfx1f_short_nf/version_0/config.yaml`
and prior configs
- vctk - `logs/nfv8_prior/version_0/config.yaml`
- perc2 - `logs/percussive2_nf_v3_prior/version_0/config.yaml`
- sfx - `logs/sfx_short_nf_v0_prior/version_0/config.yaml`
wait tf the perc2 script has vctk checkpoint paths, which configs were actually used
prior configs are the same except 20 epochs for vctk and 200 for perc2 and sfx, not sure what to think about that

[Weight Normalization: A Simple Reparameterization to Accelerate Training of Deep Neural Networks](https://arxiv.org/pdf/1602.07868)

!! the perc2 nonf config is out of date due to rerunning with `v1_phase2_vol2.yaml`, which switched eps to 1e-3
# reconstruction / uncond generation
vctk/perc/sfx
NF/no-NF
vs RAVE
# linear probing
just probing
rmod->perc transfer

fk probing doesn't mean it's controllable, bcz non-linear probes exist too
and control isn't always so straightforward, some papers say that the best decoding direction isn't necessarily the best one to change a feature
but, the method of control is easy for a found linear direction, and in non-linear probes it becomes expensive
which would be very bad for our usecase, so that's an argument

who does this anyway compare
not the conditioning papers and not the unsupervised extraction ones
# meaningful directions
vs. metadata, just qualitatively
and fidelity
vs DrumGAN
- do I report just perc? since I can compare with DrumGAN
    - report the rest if time permits
- do I report SeFa or just PCA in the VAE case?
    - their pretty much the same - I can write that
    - though that would maybe require explaining the whole SeFa with covariance weighting thing fuck

---
# tekst

Zaproponowany model został wytrenowany na każdym zestawie danych w trzech wersjach:
\begin{itemize}
    \setlength\itemsep{0em}
    \item bez \textit{Normalizing Flows},
    \item z NF posterior, oraz
    \item z NF posterior i prior.
\end{itemize}
Jako baseline dla każdego zestawu wytrenowany został również model RAVE wraz z rekurencyjną siecią prior. Na zestawie \textit{Percussive} wytrenowany został również model DrumGAN. Dla zaproponowanego modelu i RAVE przeprowadzone są dwa rodzaje ewaluacji: rekonstrukcja przykładów i bezwarunkowe generowanie syntetycznych przykładów (oznaczone jako \textit{generacja} w tabelkach). Rekonstrukcja liczy metryki na całym zbiorze testowym przepuszczonym przez auto-enkoder, zaś generacja liczy metryki na 1000 przykładach wylosowanych z rozkładu prior dla każdego modelu. 

Wszystkie modele zostały wytrenowane na karcie GeForce 1080 Ti. Tabelka~\ref{tab:per-ds-params-nonf} przedstawia wybrane parametry modeli dla poszczególnych zestawów danych w wersji bez NF, zaś tabelka~\ref{tab:per-ds-params-nf} w wersji z NF posterior. Do optymalizacji wag użyty został optymalizator Adam z parametrami $\beta_1 = 0.5$ i $\beta_2 = 0.9$. Dodatkowo. Wszystkie modele są wytrenowane na dźwięku \textit{mono} (jedno-kanałowym), podobnie jak wszystkie prace referencyjne. Do wszystkich wag zaaplikowana została normalizacja zgodnie z pracą \textit{Weight Normalization: A Simple Reparameterization to Accelerate Training of Deep Neural Networks}.

\begin{table}[htbp]
    \centering
    \caption{Parametry modeli}
    \label{tab:per-ds-params-nonf}
    \vspace{4pt}
    \begin{tabular}{lccc}
        \toprule
        \textbf{parametr} & \textbf{VCTK words} & \textbf{Percussive} & \textbf{SFX} \\
        \midrule
        capacity & \multicolumn{3}{c}{96} \\
        latent dimensions & \multicolumn{3}{c}{256} \\
        strides & \multicolumn{3}{c}{4, 4, 4, 2} \\
        initial lr & $10^{-4}$ & $10^{-5}$ & $5\cdot10^{-5}$ \\
        final lr & $5\cdot10^{-5}$ & $10^{-5}$ & $2\cdot10^{-5}$ \\
        discriminator lr & \multicolumn{3}{c}{$5\cdot10^{-6}$} \\
        FM loss weight & 20.0 & 50.0 & 20.0 \\
        adversarial loss weight & 0.5 & 1.0 & 1.0 \\
        latent loss weight & \multicolumn{3}{c}{$10^{-6} \to 10^{-4}$} \\
        STFT epsilon & $10^{-7}$ & $10^{-3}$ & $10^{-4}$ \\
        epochs & 100 & 800 & 2000 \\
        \bottomrule
    \end{tabular}
\end{table}

\begin{table}[htbp]
    \centering
    \caption{Parametry modeli}
    \label{tab:per-ds-params-nf}
    \vspace{4pt}
    \begin{tabular}{lccc}
        \toprule
        \textbf{parametr} & \textbf{VCTK words} & \textbf{Percussive} & \textbf{SFX} \\
        \midrule
        capacity & \multicolumn{3}{c}{94} \\
        latent dimensions & \multicolumn{3}{c}{256} \\
        strides & \multicolumn{3}{c}{4, 4, 4, 2} \\
        initial lr & $10^{-4}$ & $10^{-5}$ & $5\cdot10^{-5}$ \\
        final lr & $5\cdot10^{-5}$ & $10^{-5}$ & $2\cdot10^{-5}$ \\
        discriminator lr & $10^{-5}$ & $5\cdot10^{-6}$ & $5\cdot10^{-6}$ \\
        FM loss weight & 20.0 & 40.0 & 20.0 \\
        adversarial loss weight & 0.5 & 1.0 & 1.0 \\
        latent loss weight & $10^{-6} \to 10^{-4}$ & $10^{-6} \to 2\cdot10^{-4}$ & $10^{-6} \to 2\cdot10^{-4}$ \\
        STFT epsilon & $10^{-7}$ & $10^{-3}$ & $10^{-4}$ \\
        epochs & 100 & 800 & 2000 \\
        \midrule[\heavyrulewidth]
        realnvp layers & 10 & 6 & 10 \\
        realnvp hidden size & \multicolumn{3}{c}{64} \\
        \bottomrule
    \end{tabular}
\end{table}

Parametry komponentu generującego filtrowany szum są te same we wszystkich modelach: trzy warstwy ukryte po 128 jednostek i 6 wartości określających docelową charakterystykę.

Tabelki~\ref{tab:main-results} przedstawiają wyniki rekonstrukcji i generacji. \textbf{no-NF} oznacza wersję bez \textit{normalizing flows}, \textbf{NF} z NF jako rozkładem posterior, a \textbf{NF+prior} z dodatkowym użyciem NF jako rozkład prior. Dla porównania, wyniki \textit{Vendi} na samych zbiorach testowych to: 6.1212 dla \textit{VCTK words}, 21.6521 dla \textit{Percussive} oraz 39.0917 dla \textit{SFX}.

\begin{table}[htbp]
    \centering
    \caption{Wyniki rekonstrukcji i bezwarunkowej generacji}
    \label{tab:main-results}
    \vspace{4pt}
    \begin{subtable}[t]{\textwidth}
        \centering
        \caption{VCTK}
        \label{tab:main-vctk}
        \vspace{4pt}
        \begin{tabular}{lcccc}
            \toprule
            \textbf{model} & \textbf{tryb} & \textbf{KID} & \textbf{FAD} & \textbf{Vendi} \\
            \midrule
            no-NF & rekonstrukcja & 0.0015 & 0.0248 & 5.9449 \\
            NF & rekonstrukcja & 0.0015 & 0.0247 & 6.1620 \\
            RAVE & rekonstrukcja & 0.0017 & 0.0268 & 5.9428 \\
            \midrule[\heavyrulewidth]
            no-NF & generacja & 0.0534 & 0.5886 & 3.9058 \\
            NF & generacja & 0.0549 & 0.6049 & 3.5694 \\
            RAVE+prior & generacja & 0.0535 & 0.5804 & 4.3253 \\
            NF+prior & generacja & 0.0115 & 0.1491 & 5.6436 \\
            \bottomrule
        \end{tabular}
    \end{subtable}
    \vspace{12pt}
    \begin{subtable}[t]{\textwidth}
        \centering
        \caption{Percussive}
        \label{tab:main-percussive}
        \vspace{4pt}
        \begin{tabular}{lcccc}
            \toprule
            \textbf{model} & \textbf{tryb} & \textbf{KID} & \textbf{FAD} & \textbf{Vendi} \\
            \midrule
            no-NF & rekonstrukcja & 0.0045 & 0.1569 & 15.2375 \\
            NF & rekonstrukcja & 0.0040 & 0.1271 & 15.2508 \\
            RAVE & rekonstrukcja & 0.0126 & 0.3082 & 16.4382 \\
            \midrule[\heavyrulewidth]
            no-NF & generacja & 0.0537 & 0.8103 & 10.0454 \\
            NF & generacja & 0.0504 & 0.7732 & 10.2943 \\
            RAVE & generacja & 0.0350 & 0.7139 & 14.0476 \\
            NF+prior & generacja & 0.0180 & 0.3539 & 9.7582 \\
            DrumGAN & generacja & 0.0197 & 0.3647 & 15.6899 \\
            \bottomrule
        \end{tabular}
    \end{subtable}
    \vspace{12pt}
    \begin{subtable}[t]{\textwidth}
        \centering
        \caption{SFX}
        \label{tab:main-sfx}
        \vspace{4pt}
        \begin{tabular}{lcccc}
            \toprule
            \textbf{model} & \textbf{tryb} & \textbf{KID} & \textbf{FAD} & \textbf{Vendi} \\
            \midrule
            no-NF & rekonstrukcja & 0.0066 & 0.1627 & 27.2088 \\
            NF & rekonstrukcja & 0.0072 & 0.1754 & 26.9966 \\
            RAVE & rekonstrukcja & 0.0115 & 0.2590 & 24.6911 \\
            \midrule[\heavyrulewidth]
            no-NF & generacja & 0.0685 & 0.9353 & 4.6449 \\
            NF & generacja & 0.0693 & 0.9768 & 4.1123 \\
            RAVE & generacja & 0.0288 & 0.5165 & 18.2279 \\
            NF+prior & generacja & 0.0217 & 0.3678 & 16.0714 \\
            \bottomrule
        \end{tabular}
    \end{subtable}
\end{table}

Kluczową do osiągnięcia dobrej jakości dźwięku okazała się modyfikacja funkcji straty opartej na STFT. Liczy ona odległość L2 między samymi spektrogramami oraz L1 między ich logarytmami. Ponieważ absolutna wartość pochodnej logarytmu rośnie wraz ze zbliżaniem się do zera, cały logarytmiczny składnik przykłada większą wagę do cichych obszarów spektrogramu. W ogólności jest to pożądany efekt, mający na celu wzmocnienie sygnału kierującego w stronę precyzyjnej rekonstrukcji cichszych szczegółów. W przypadku zupełnej może to jednak powodować, że sygnałem dominującym jest ten w stronę idealnej rekonstrukcji ciszy, która nie będzie już zupełnie słyszalna. Do spektrogramu, przed wzięciem logarytmu, dodawany jest $\epsilon = 10^{-7}$ żeby uniknąć nieskończonych wartości. W praktyce wartość ta ogranicza maksymalną wagę gradientu, jaka może z niego wyjść dla danego elementu spektrogramu:
$$\frac{\partial}{\partial x}|\log(x + \epsilon) - \log(y + \epsilon)| = \frac{\operatorname{sign}(x - y)}{x + \epsilon},$$
co daje maksymalną wartość absolutną równą $\frac{1}{\epsilon}$. Zwiększenie $\epsilon$ do wartości takich jak $10^{-4}$ czy $10^{-3}$ zmnieszja ten limit i powoduje, że gradient w mniejszym stopniu zmusza model do idealnej rekonstrukcji ciszy. Prawodpodobne jest to, że w danych RAVE, z powodu małej ilości ciszy, efekt ten nie był zauważalny. W przypadku tej pracy przykłady często jednak składają się w dużej części z ciszy, co przy wartości $\epsilon = 10^{-7}$ w znaczącym stopniu destabilizowało trening. Wpływ zwiększenia $\epsilon$ na trening modelu RAVE nie został zbadany.

\section*{Analiza przestrzeni ukrytej}

Oprócz jakości generowanych przykładów, w tej pracy przeprowadzona jest analiza struktury przestrzeni ukrytej. Z punktu widzenia docelowego zastosowania, łatwa kontrola interpetowalnych i wysokopoziomowych aspektów dźwięku jest istotna. W tym celu praca ta bada na ile różne cechy przykładów są liniowo zakodowane w przestrzeni ukrytej oraz na ile najbardziej istotne kierunki w przestrzeni ukrytej odpowiadają jakimś interpretowalnym cechom.

\subsection{Sondowanie liniowe}

Dla każdego zestawu, na przykładach testowych zakodowanych w przestrzeni ukrytej, wytrenowany został zestaw klasyfikatorów liniowych - jeden dla każdego wymiaru metadanych. Wysoki wynik klasyfikatora oznacza, że dana cecha jest liniowo dekodowalna, co następnie może przełożyć się możliwość jej kontroli.

(TODO wyniki)

\subsection{Najbardziej znaczące kierunki}

Ten eksperyment został przeprowadzony wyłącznie na zbiorze Percussive, również dla modelu DrumGAN w celu porównania. Dla VAE, najbardziej znaczące kierunki w przestrzeni ukrytej zostały wyciągnięte na dwa sposoby:
\begin{itemize}
    \setlength\itemsep{0em}
    \item \textit{Semantic Factorization} (SeFa), oraz
    \item PCA przykładach zakodowanych w przestrzeni ukrytej.
\end{itemize}
Dla DrumGAN użyte zostało tylko SeFa, jako że model ten nie umożliwia kodowania przykładów i PCA nie miałoby sensu. Dla VAE, obie metody okazały się dawać prawie identyczne wyniki, więc raportowane są tylko te z SeFa. Użyta została wersja najlepiej radząca sobie w bezwarunkowej generacji, \textbf{NF+prior}.

(TODO wyniki)
