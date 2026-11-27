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

feature coherence maybe?
# meaningful directions
vs. metadata, just qualitatively
and fidelity
vs DrumGAN
- do I report just perc? since I can compare with DrumGAN
    - report the rest if time permits
- do I report SeFa or just PCA in the VAE case?
    - their pretty much the same - I can write that
    - though that would maybe require explaining the whole SeFa with covariance weighting thing fuck

ok I def have to note there's much to do w.r.t. latent space analysis
for now it's just a "promising direction"

  │                                │ DrumGAN (SeFa) │ VAE        │
  │ control ≥1 descriptor          │ 3 / 10     │ 10 / 10        │
  │ Median top |ρ| / consistency   │ 0.59 / 90% │ 0.97 / 100%    │
  │ Median effect per σ            │ 0.05       │ 0.18           │
  │ Strongest direction's effect   │ 0.15 (dir 0) │ 0.25 (dir 1) │
  │ Distinct axes                  | 3          │ 7              │
  │ Median profile overlap         │ 0.71       │ 0.80           │

---
# tekst

Zaproponowany model został wytrenowany na każdym zestawie danych w trzech wersjach:
\begin{itemize}
    \setlength\itemsep{0em}
    \item bez \textit{Normalizing Flows},
    \item z NF posterior, oraz
    \item z NF posterior i prior.
\end{itemize}
Jako baseline dla każdego zestawu wytrenowany został również model RAVE wraz z rekurencyjną siecią prior. Na zestawie \textit{Percussive} wytrenowany został również model DrumGAN. Dla zaproponowanego modelu i dla RAVE przeprowadzone są dwa rodzaje ewaluacji: rekonstrukcja przykładów i bezwarunkowe generowanie syntetycznych przykładów (oznaczone jako \textit{generacja} w tabelkach). Rekonstrukcja liczy metryki na całym zbiorze testowym przepuszczonym przez autoenkoder, zaś generacja liczy metryki na 1000 przykładach wylosowanych z rozkładu prior dla każdego modelu.

Wszystkie modele zostały wytrenowane na karcie GeForce 1080 Ti. Tabelka~\ref{tab:per-ds-params-nonf} przedstawia wybrane parametry modeli dla poszczególnych zestawów danych w wersji bez NF, zaś tabelka~\ref{tab:per-ds-params-nf} w wersji z NF posterior. Do optymalizacji wag użyty został optymalizator Adam z parametrami $\beta_1 = 0.5$ i $\beta_2 = 0.9$. Dodatkowo, wszystkie modele są wytrenowane na dźwięku \textit{mono} (jedno-kanałowym), podobnie jak wszystkie prace referencyjne. Do wszystkich wag zaaplikowana została normalizacja zgodnie z pracą \textit{Weight Normalization: A Simple Reparameterization to Accelerate Training of Deep Neural Networks}. Ilość epok dla każdego modelu różni się w takim stopniu z powodu dużych różnic z liczebnościach ich zestawów treningowych.

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

W kwestii rekonstrukcji, wyniki pokazują, że proponowany model osiąga lepszą jakość niż referencyjna implementacja RAVE. Na zbiorze \textit{VCTK words} różnica ta jest minimalna, jednak na \textit{Percussive} i \textit{SFX} jest już znacząca. Dodanie NF posterior praktycznie nie wpływa na wynik, co odrzuca wcześniej postawioną tezę, że bardziej ekspresywny approximate posterior pozytywnie wpłynie na model.

W bezwarunkowej generacji, wyniki pokazują, że:
\begin{itemize}
    \setlength\itemsep{0em}
    \item NF posterior praktycznie nie wpływa na jakość/różnorodność;
    \item generacja ze standardowego rozkładu prior $\mathcal{N}(0, \, 1)$ daje podobny wynik co RAVE z rekurencyjną siecią prior na zestawie \textit{VCTK words} i gorszy na \textit{Percussive}/\textit{SFX}, zarówno pod względem jakości jak i różnorodności;
    \item generacja z dodatkowego rozkładu NF prior znacząco poprawia jakość względem RAVE w każdym z przypadków, tracąc jednak w różnorodności na zbiorze \textit{Percussive}.
\end{itemize}
O ile bezpiecznie można stwierdzić, że NF posterior nie poprawia w żadnym przypadku działania modelu, dodatkowy rozkład NF prior jest kluczowym elementem dla generowania realistycznie brzmiących przykładów o wysokiej jakości. Oznacza to, że prawdziwy rozkład posterior odbiega jednak znacząco od $\mathcal{N}(0, \, 1)$.

\section*{Analiza przestrzeni ukrytej}

Oprócz jakości generowanych przykładów, przeprowadzona w tej pracy jest analiza struktury przestrzeni ukrytej. Z punktu widzenia docelowego zastosowania, łatwa kontrola interpetowalnych i wysokopoziomowych aspektów dźwięku jest istotna. W tym celu zbadane zostało na ile różne cechy przykładów są liniowo zakodowane w przestrzeni ukrytej oraz na ile najbardziej istotne kierunki w przestrzeni ukrytej odpowiadają jakimś interpretowalnym cechom. Semantycznie znaczące kierunki mogą się następnie przełożyć na kontrolę.

Na zbiorach \textit{Percussive} i \textit{SFX}, z powodu braku metadanych (oprócz kategorii instrumentu w \textit{Percussive}, chociaż tylko część przykładów jest zaanotowana), użyte są deskryptory obliczone za pomocą biblioteki \texttt{essentia}. Tabelka~\ref{tab:essentia-features} przedstawia listę 18 użytych cech wraz z krótkim opisem. Cechy z dwoma wariantami \texttt{Std} i \texttt{Mean} to te liczone w oknach - \texttt{Mean} oznacza średnią tych pomiarów, a \texttt{Std} odchylenie standardowe.

\begin{table}[htbp]
    \centering
    \caption{Deskryptory \texttt{essentia}}
    \label{tab:essentia-features}
    \vspace{4pt}
    \begin{tabular}{lc}
        \toprule
        \textbf{deskryptor} & \textbf{opis} \\
        \midrule
        loudness & Głośność (Steven's power-law) \\
        decayStrength & Ile energii jest w ogonie dźwięku \\
        spectralCentroidMean & Środek ciężkości widma (średnia) \\
        rms & Głośność (średnia kwadratowa) \\
        spectralComplexityMean & Liczba szczytów widma (średnia) \\
        duration90pctEnergy & Czas do osiągnięcia 90% energii \\
        energyCentroid & Położenie środka ciężkości energii \\
        spectralComplexityStd & Zmiennośc liczby szczytów widma \\
        spectralFlatMean & Płaskość widma (średnia) \\
        pConfidenceMean & Pewność estymacji wysokości dźwięku (średnia) \\
        logAttack & logarytm czasu ataku \\
        dynamicComplexity & Średnie wahania głośności \\
        pSalienceMean & Wyrazistość wysokości dźwięku (średnia) \\
        spectralCentroidStd & Zmienność środka ciężkości widma \\
        pSalienceStd & Zmienność wyrazistości wysokości dźwięku \\
        pConfidenceStd & Zmienność pewności estymacji wysokości dźwięku \\
        spectralFlatStd & Zmienność płaskości widma \\
        inharmonicity & odchylenie częstotliwości składowych od szeregu harmonicznego \\
        \bottomrule
    \end{tabular}
\end{table}

\subsection{Sondowanie liniowe}

Dla każdego zestawu, na przykładach testowych zakodowanych w przestrzeni ukrytej, wytrenowany został zestaw klasyfikatorów liniowych - jeden dla każdego wymiaru metadanych. Wysoki wynik klasyfikatora oznacza, że dana cecha jest liniowo dekodowalna, co następnie może przełożyć się możliwość jej kontroli. Tabelki~\ref{tab:probing-results} przedstawiają wyniki sond na każdym zestawie. Wersje z i bez NF dawały praktycznie takie same wyniki, więc przedstawione liczby są dla modeli \textbf{z NF}. Dla zestawów \textit{Percussive} i \textit{SFX} przedstawione są głównie wyniki dla cech wyciągniętych za pomocą \texttt{essentia}, gdyż \textit{SFX} nie zawiera dodatkowych metadanych, a w \textit{Percussive} jest to wyłącznie kategoria instrumentu. \textit{Zrównoważona dokładność} to średnia dokładności w obrębie każdej klasy (równe średniemu \textit{recall}).

\begin{table}[htbp]
    \centering
    \caption{Wyniki sondowania liniowego.}
    \label{tab:probing-results}
    \vspace{4pt}
    \begin{subtable}[t]{\textwidth}
        \centering
        \caption{VCTK}
        \label{tab:probing-vctk}
        \vspace{4pt}
        \begin{tabular}{ll}
            \toprule
            \textbf{cecha} & \textbf{wynik} \\
            \midrule
            Płeć & 0.996 AUC \\
            Długość & 0.962 $R^2$ \\
            Akcent & 0.136 dokładność \\
            Słowo & 0.256 zrównoważona dokładność \\
            \bottomrule
        \end{tabular}
    \end{subtable}
    \vspace{12pt}
    \begin{subtable}[t]{\textwidth}
        \centering
        \caption{Percussive}
        \label{tab:probing-percussive}
        \vspace{4pt}
        \begin{tabular}{ll}
            \toprule
            \textbf{cecha} & \textbf{wynik} \\
            \midrule
            kategoria instrumentu & 0.947 dokładność \\
            decayStrength & 0.85 $R^2$ \\
            rms & 0.78 $R^2$ \\
            loudness & 0.71 $R^2$ \\
            spectralComplexityMean & 0.61 $R^2$ \\
            spectralComplexityStd & 0.60 $R^2$ \\
            energyCentroid & 0.48 $R^2$ \\
            spectralCentroidMean & 0.46 $R^2$ \\
            pSalienceMean & 0.45 $R^2$ \\
            dynamicComplexity & 0.37 $R^2$ \\
            spectralFlatMean & 0.33 $R^2$ \\
            logAttack & 0.31 $R^2$ \\
            pConfidenceMean & 0.19 $R^2$ \\
            spectralFlatStd & 0.17 $R^2$ \\
            pConfidenceStd & 0.12 $R^2$ \\
            spectralCentroidStd & 0.09 $R^2$ \\
            pSalienceStd & -0.06 $R^2$ \\
            duration90pctEnergy & -0.074 $R^2$ \\
            inharmonicity & -0.26 $R^2$ \\
            \bottomrule
        \end{tabular}
    \end{subtable}
    \vspace{12pt}
    \begin{subtable}[t]{\textwidth}
        \centering
        \caption{SFX}
        \label{tab:probing-sfx}
        \vspace{4pt}
        \begin{tabular}{ll}
            \toprule
            \textbf{cecha} & \textbf{wynik} \\
            \midrule
            duration90pctEnergy & 0.78 $R^2$ \\
            loudness & 0.75 $R^2$ \\
            decayStrength & 0.71 $R^2$ \\
            spectralCentroidMean & 0.68 $R^2$ \\
            rms & 0.66 $R^2$ \\
            spectralComplexityMean & 0.59 $R^2$ \\
            energyCentroid & 0.45 $R^2$ \\
            spectralComplexityStd & 0.45 $R^2$ \\
            spectralFlatMean & 0.39 $R^2$ \\
            pConfidenceMean & 0.37 $R^2$ \\
            logAttack & 0.33 $R^2$ \\
            dynamicComplexity & 0.31 $R^2$ \\
            pSalienceMean & 0.20 $R^2$ \\
            spectralCentroidStd & 0.16 $R^2$ \\
            pSalienceStd & 0.07 $R^2$ \\
            pConfidenceStd & 0.01 $R^2$ \\
            spectralFlatStd & 0.01 $R^2$ \\
            inharmonicity & -0.06 $R^2$ \\
            \bottomrule
        \end{tabular}
    \end{subtable}
\end{table}

Dla zestawu \textit{VCTK} widać, że:
\begin{itemize}
    \setlength\itemsep{0em}
    \item płeć i długość słowa są bardzo wyraźnie zakodowane w przestrzeni ukrytej;
    \item akcent jest praktycznie niedekodowalny - osiągnięta dokładność jest niewiele większa niż losowe zgadywanie (0.125 dokładności);
    \item informację o słowie, choć nie idealnie, da się wyczytać lepiej niż przy losowym zgadywaniu (0.006 dokładności), co świadczy o tym, że w jakimś stopniu jest dekodowalna.
\end{itemize}
Dla płci, ponieważ jest to klasyfikacja binarna, zamiast regresji logistycznej można policzyć kierunek prowadzący od średniego punktu z jednej klasy do średniego punktu drugiej - \textit{mean-difference direction}. Rzutowanie punktu na ten kierunek samo w sobie osiąga już 0.983 AUC, co świadczy o bardzo wyraźnym rozdzieleniu tych dwóch klas w przestrzeni ukrytej. Klasyfikacja słów została przeprowadzona na podzbiorze przefiltrowanym tak, żeby:
\begin{itemize}
    \setlength\itemsep{0em}
    \item minimalna długość nagrania była równa 0.3 sekundy;
    \item dla każdego słowa było minimum 15 przykładów;
    \item dla każdego słowa było minimum 5 różnych mówców wypowiadających te słowo.
\end{itemize}
Dało to zbiór 3483 nagrań, mających 156 różnych słów wypowiadanych przez 109 różnych mówców. Ewaluacja została przeprowadzona za pomocą kroswalidacji grupowanej w ten sposób, żeby dany mówca był wyłącznie w zbiorze treningowym lub testowym, co uniemożliwia klasyfikatorowi poleganie na cechach samego mówcy. Zrównoważona dokładność na poziomie 0.256 świadczy o tym, że niezależnie od innych cech, samo słowo jest w jakimś stopniu zakodowane w przestrzeni ukrytej. 

Dla \textit{Percussive} i \textit{SFX}, wśród najlepiej dekodowalnych kierunków są głównie deskryptory długości, głośności oraz w troche mniejszym stopniu jasności brzmienia. Ponieważ sama dekodowalność nie świadczy jeszcze o możliwości kontroli, kolejny eksperyment bada relację najistotniejszych kierunków przestrzeni ukrytej z deskryptorami.

\subsection{Najbardziej znaczące kierunki}

Ten eksperyment został przeprowadzony wyłącznie na zbiorze Percussive, wraz z modelem DrumGAN dla porównania. Dla proponowanego VAE, najbardziej znaczące kierunki w przestrzeni ukrytej zostały wyciągnięte na dwa sposoby:
\begin{itemize}
    \setlength\itemsep{0em}
    \item \textit{Semantic Factorization} (SeFa), oraz
    \item PCA na przykładach zakodowanych w przestrzeni ukrytej.
\end{itemize}
Dla DrumGAN użyte zostało tylko SeFa, jako że model ten nie umożliwia kodowania przykładów i PCA nie miałoby sensu. Ponieważ dla VAE obie metody okazały się dawać prawie identyczne wyniki, raportowane są tylko te z PCA na przykładach. Użyta została wersja najlepiej radząca sobie w bezwarunkowej generacji, \textbf{NF+prior}.

\begin{figure}[t]
  \centering
  \includegraphics{figures/udzial_wariancji.pdf}
  \caption{Udział wariancji dla każdego z 10 najistotniejszych kierunków dla DrumGAN i dla proponowanego VAE. Wyraźnie widać, że VAE rozkłada wariancje równomiernie po większej ilości kierunków.}
  \label{fig:variance-share}
\end{figure}

Wykres~\ref{fig:variance-share} przedstawia procent wyjaśnianej wariancji dla każdego z 10 najbardziej znaczących kierunków obu modeli. Widać na nim, że VAE znacznie równiej dystrybuuje ją po kierunkach, gdzie w DrumGAN jeden z nich dominuje. W praktyce przekłada się to na więcej semantycznie znaczących kierunków dla VAE. Dla każdego kierunku następnie zostało wyestymowane czy istnieje jakiś deskryptor który jest wyraźnie przez ten kierunek kontrolowany. Użyta metoda jest zainspirowana przez tę użytą dla SeFa, nazwaną \textit{re-scoring analysis}. Dla danego kierunku i deskryptora algorytm jest następujący:
\begin{itemize}
    \setlength\itemsep{0em}
    \item losowane jest 10 przykładów w przestrzeni ukrytej;
    \item każdy z nich jest modyfikowany o 4 kroki w zgodnie z danym kierunkiem i o 4 kroki w przeciwnym kierunku, od $-3\sigma$ do $+3\sigma$;
    \item dla każdej z modyfikacji (w sumie 8x10 przykładów) liczona jest wartość deskryptora;
    \item dla każdego z 10 przykładów liczona jest korelacja $\rho_i$ deskryptora z wielkością kroku, dając monotoniczność zmiany;
    \item jeśli średnia $|\text{avg}(\rho_i)|$ jest większa niż 0.7 i żadna z $\rho_i$ nie jest większa niż 0.1 w przeciwnym kierunku (i.e. wszystkie są \textit{spójne}), uznane zostaje, że deskryptor jest powiązany z kierunkiem.  
\end{itemize}
Tabelka~\ref{tab:variance-stats} przedstawia wyniki tak policzonych dopasowań na 10 najistotniejszych kierunkach dla każdego modelu. Spójność dla danego kierunku i deskryptora oznacza procent spójnych wylosowanych przykładów z 10. Mediana $\rho$ i mediana spójności są liczone na przestrzeni wszystkich 10 kierunków, nawet jeśli nie mają dopasowanego deskryptora.

\begin{table}[htbp]
    \centering
    \caption{Liczba znaczących kierunków odpowiadających jakiemuś deskryptorowi.}
    \label{tab:variance-stats}
    \vspace{4pt}
    \begin{tabular}{lcc}
        \toprule
        \textbf{statystyka} & \textbf{DrumGAN (SeFa)} & \textbf{VAE (PCA)} \\
        \midrule
        kontrolujące 1+ deskryptorów & 3 & 10 \\
        mediana maksymalnego $\rho$ & 0.59 & 0.97 \\
        mediana spójności & 90% & 100% \\
        \bottomrule
    \end{tabular}
\end{table}

W porównaniu z DrumGAN, VAE znajduje znacząco więcej istotnych kierunków które w wyraźny sposób kontrolują jakieś deskryptory. Patrząc na medianę $\rho$, korelacje kierunków DrumGAN są o wiele niższe, a niektóre z nich okazjonalnie modyfikują deskryptory w przeciwnym kierunku zależnie od wylosowanego punktu w przestrzeni. Kierunki w VAE są jednak w dużym stopniu splątane z deskryptorami, co znaczy, że modyfikują ich kilka naraz - przykładowo, kierunek 4 kontroluje jednocześnie jasność i głośność brzmienia. Tabelka~\ref{tab:essentia-entanglement} listuje deskryptory modyfikowane przez każdy z kierunków wraz z siłą i zwrotem. Widać, że kierunki znalezione przez VAE są znacząco wyraźniej powiązane z różnymi deskryptorami, jednak dla danego kierunku deskryptory pokrywają różne cechy dźwięku.

\begin{table}[htbp]
    \centering
    \caption{Deskryptory modyfikowane przez każdy z istotnych kierunków w przestrzeni ukrytej, który jest silnie skorelowany z przynajmniej jednym deskryptorem. Każdy z deskryptor ma przy sobie wartość korelacji, i.e. siłę z jaką kierunek go modyfikuje, wraz ze zwrotem (+ oznacza, że deskryptor rośnie wraz z trawersem kierunku, - oznacza, że maleje)}
    \label{tab:essentia-entanglement}
    \vspace{4pt}
    \begin{subtable}[t]{\textwidth}
        \centering
        \caption{DrumGAN}
        \label{tab:essentia-drumgan}
        \vspace{4pt}
        \begin{tabular}{ll}
            \toprule
            \textbf{indeks kieruneku} & \textbf{deskryptory} \\
            \midrule
            0 & spectralCentroidMean (+0.77), pConfidenceMean (-0.71) \\
            2 & energyCentroid (-0.72) \\
            6 & spectralCentroidMean (+0.76) \\
            \bottomrule
        \end{tabular}
    \end{subtable}
    \vspace{12pt}
    \begin{subtable}[t]{\textwidth}
        \centering
        \caption{VAE}
        \label{tab:essentia-slay}
        \vspace{4pt}
        \begin{tabular}{ll}
            \toprule
            \textbf{indeks kieruneku} & \textbf{deskryptory} \\
            \midrule
            0 & spectralComplexityStd (-0.92), spectralComplexityMean (-0.91), pSalienceMean (-0.91), spectralFlatMean (-0.81) \\
            1 & spectralCentroidMean (+1.00), spectralFlatMean (+1.00), duration90pctEnergy (-0.98), energyCentroid (-0.91), pConfidenceMean (-0.83), logAttack (-0.80), inharmonicity (-0.75), pSalienceMean (+0.74) \\
            2 & spectralComplexityMean (-0.98), spectralComplexityStd (-0.98), loudness (-0.96), rms (-0.96), decayStrength (-0.96), pConfidenceStd (-0.74), spectralFlatMean (+0.71) \\
            3 & spectralFlatMean (+0.99), spectralCentroidStd (+0.94), spectralCentroidMean (+0.92), spectralFlatStd (+0.87), spectralComplexityStd (+0.87), decayStrength (+0.84), spectralComplexityMean (+0.74), pConfidenceMean (-0.74) \\
            4 & spectralCentroidMean (+1.00), spectralFlatMean (+1.00), spectralComplexityMean (-1.00), spectralComplexityStd (-0.99), energyCentroid (-0.98), duration90pctEnergy (-0.91), logAttack (-0.83), loudness (-0.78), rms (-0.78), pConfidenceMean (-0.75) \\
            5 & duration90pctEnergy (-0.97), spectralFlatStd (-0.96), spectralCentroidStd (-0.94), spectralComplexityStd (-0.91), spectralComplexityMean (-0.89), energyCentroid (-0.87), logAttack (-0.84) \\
            6 & energyCentroid (+0.97), duration90pctEnergy (+0.90), spectralCentroidMean (+0.89), spectralFlatStd (+0.89), spectralFlatMean (+0.88), logAttack (+0.81) \\
            7 & spectralCentroidMean (+1.00), duration90pctEnergy (-1.00), spectralFlatMean (+0.99), energyCentroid (-0.96), pSalienceMean (+0.95), pConfidenceMean (-0.77), spectralComplexityMean (-0.74) \\
            8 & spectralComplexityMean (+0.92), spectralFlatStd (-0.91), energyCentroid (+0.84), spectralCentroidStd (-0.82) \\
            9 & decayStrength (+0.85), spectralComplexityStd (+0.85), logAttack (+0.82), loudness (+0.76), rms (+0.76) \\
            \bottomrule
        \end{tabular}
    \end{subtable}
\end{table}