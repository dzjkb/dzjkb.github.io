## Laion CLAP
krótki opis z papera
## FAD/KID
[[Frechet Audio Distance]] - paper + nowsze uwagi z fadtk
[[Kernel Inception Distance]]
- wprowadzony w DrumGAN?

implicit jakość/różnorodność, ciężko o niereferencyjne metryki jakości

KID papers
- [original paper](https://arxiv.org/pdf/0805.2368)
- [closed form for autoencoders](https://arxiv.org/pdf/1901.03227)
## Vendi
krótki opis z papera
czemu zależy nam na różnorodności?
## latent space metrics (fidelity?)
fidelity z RAVE
coś jeszcze?

---
# text

Jakość zaproponowanego rozwiązania ewaluuję za pomocą trzech metryk:
\begin{enumerate}
    \setlength\itemsep{0em}
    \item \textbf{Frechet Audio Distance},
    \item \textbf{Kernel Inception Distance},
    \item \textbf{Vendi score}.
\end{enumerate}
Każda z nich działa nie bezpośrednio na fali dźwiękowej, lecz na jej \textbf{embeddingach} kodujących nisko i wysokopoziomowe własności. Do embeddingów wykorzystane są modele z pracy \textit{Large Scale Contrastive Language-Audio Pretraining with Feature Fusion and Keyword-to-caption Augmentation}, w skrócie nazwane \textit{embeddingi CLAP}.

\section*{CLAP}

Praca \textit{Large Scale Contrastive Language-Audio Pretraining with Feature Fusion and Keyword-to-caption Augmentation} z 2024 roku, zainspirowana podobnymi podejściami w dziedzinie obrazu, porusza problem stworzenia uniwersalnych multi-modalnych embeddingów dla dźwięku, bazując na przykładach wraz z ich opisami. Oznacza to pewnego rodzaju mapowanie konceptów z języka naturalnego w dźwięk (i na odwrót), co następnie może umożliwić na przykład semantyczne wyszukiwanie nagrań z jakiejś bazy na podstawie podanej frazy. Jednym z głównych osiagnięć pracy jest stworzenie zestawu danych \textit{LAION-Audio-630K}, zestawu sparowanych klipów audio z opisami, który jest większy o rząd wielkości niż największy dotychczasowy zestaw tego typu. Taki rozmiar pozwolił na wytrenowanie modeli wysokiej jakości, które autorzy następnie ewaluowali na trzech zadaniach: wyszukiwanie nagrania z tekstu, klasyfikacja nagrania bez dodatkowego uczenia (\textit{zero-shot classification}) i klasyfikacja z dodatkowym krokiem uczenia. Na trzech zestawach danych, CLAP przebił najlepszy dotychczasowy wynik w ustawieniu \textit{zero-shot}, co świadczy o silnej zdolności generalizacji.

Autorzy CLAP udostępnili kilka już wytrenowanych modeli, każdy na innej kombinacji zestawów danych, stworzonych pod inne zastosowania. W tej pracy użyte są dwa:
\begin{enumerate}
    \setlength\itemsep{0em}
    \item \texttt{music\_audioset\_epoch\_15\_esc\_90.14.pt} - do muzycznych zestawów danych, oraz
    \item \texttt{music\_speech\_epoch\_15\_esc\_89.25.pt} - do zestawów z ludzkim głosem.
\end{enumerate}

\section*{Frechet Audio Distance}

\textit{Frechet Audio Distance} (FAD) to metryka jakości dźwięku, zainspirowana stosowaną w dziedzinie przetwarzania obrazu metryką \textit{Frechet Inception Distance}. Do jej obliczenia wymagany jest zbiór referencyjny oraz model pozwalający na stworzenie embeddingów z dźwięku. Z założenia służy do ewaluacji całego zestawu wygenerowanych przykładów, chociaż w szczególności zbiór ewaluowany może zawierać tylko jeden przykład. W oryginalnym sformułowaniu, FAD działa w następujący sposób:
\begin{enumerate}
    \setlength\itemsep{0em}
    \item dla każdego przykładu ze zbioru referencyjnego i ewaluowanego liczony jest embedding - przedostatnia warstwa klasyfikatora VGGish;
    \item do obu zbiorów dopasowane zostają wielowymiarowe rozkłady normalne;
    \item liczona jest odległość \textit{Frechet distance} między rozkładami, co daje ostateczną wartość metryki.
\end{enumerate}
Odległość \textit{Frechet distance} między dwoma zbiorami jest zdefiniowana jako
$$\mathbf{F}(\mathcal{N}_r, \mathcal{N}_e) = \|\mu_r - \mu_e\|^2 + tr\left(\Sigma_r + \Sigma_e - 2\sqrt{\Sigma_r \Sigma_e}\right)$$
gdzie \texttt{r} oznacza parametry zbioru referencyjnego, \texttt{e} ewaluacyjnego a \texttt{tr} to ślad macierzy. W praktyce, tak zdefiniowana metryka implicite mierzy również różnorodność danego zbioru, co należy mieć na uwadzę interpretując wyniki.

FAD ma kilka słabych punktów:
\begin{enumerate}
    \setlength\itemsep{0em}
    \item zbiór referencyjny - zakłada, że przykłady wysokiej jakości to takie bliskie zbiorowi referencyjnemu, przez co może źle ocenić przykłady odległe, a mimo to jakościowe;
    \item rozmiar zbioru - ocena jakości jest zależna od rozmiaru ewaluowanego zbioru;
    \item gaussowskie założenie - podobieństwo zbiorów może być przekłamane, jeśli embeddingi nie są wcale rozłożone wedle
\end{enumerate}
Praca \textit{Adapting Frechet Audio Distance For Generative Music Evaluation} z 2024 roku spróbowała zmierzyć i zaadresować pierwsze dwa problemy (trzeci jest zaadresowany poprzez omówioną później metrykę \textit{Kernel Inception Distance}). Badając korelację między \textit{Mean Opinion Score} a FAD z użyciem różnych kombinacji modeli i zestawów referencyjnych, wykazała, że wybór ten może znacznie wpływać na wynik. Ewaluacje z użyciem CLAP radziły sobie najlepiej w każdym z eksperymentów, zaś wybór zbioru referencyjnego przekładał się na nacisk na inne cech muzyczne, promując na przykład jedne gatunki bardziej niż inne. W celu korekty FAD dla różnych rozmiarów zbioru, autorzy - znów zainspirowani rozwiązaniami w dziedzinie obrazu - wprowadzili metrykę $FAD_{\infty}$. Twórcy $FID_{\infty}$ opierali ją na obserwacjach, że:
\begin{enumerate}
    \setlength\itemsep{0em}
    \item FID maleje wraz ze wzrostem rozmiaru $N$;
    \item charakterystyka zależności od rozmiaru różni się między modelami;
    \item w nieskończoności $FID_{N \to \infty}$ zbiega do skończonej wartości.
\end{enumerate}
W związku z tym, analogicznie, metrykę $FAD_{\infty}$ definiuje się następująco:
\begin{enumerate}
    \setlength\itemsep{0em}
    \item ze zbioru testowego losowane (ze zwracaniem) jest wiele podzbiorów o różnych, nie powtarzających się rozmiarach $N_i \leq N$;
    \item za pomocą liniowej regresji dopasowywana jest prosta do punktów $(FAD_{i}, \: 1/N_i)$;
    \item wartość dopasowanej prostej w punkcie 0 daje wynik $FAD_{\infty}$.
\end{enumerate}
Autorzy pokazują, że wartości $FAD_i$  dla danej kombinacji modelu i zestawu referencyjnego są dobrze modelowane przez regresję liniową, na co wskazuje wysokie $R^2$ dopasowania.

O ile zamysł metryk FID i FAD był taki, żeby mierzyć jakość, z natury tego w jaki sposób są zdefiniowane mierzą również różnorodność przykładów. Gaussowski rozkład dopasowany do przykładów będących wysokiej jakości, lecz bardzo podobnych do siebie, będzie naturalnie miał mniejszą wariancję i większe \textit{Frechet distance} do większego i bardziej zróżnicowanego zbioru referencyjnego. W kontekście tej pracy, mierzenie różnorodności jest to pożądaną własnością tej metryki.

\section*{Kernel Inception Distance}

Drugą metryką, koncepcyjnie zbliżoną do FAD i również zapożyczoną z dziedziny przetwarzania obrazu, jest \textit{Kernel Inception Distance} (KID). Tak jak FAD, liczy ona odległość próbki ewaluowanych przykładów od zbioru referencyjnego w przestrzeni embeddingów. Użytą miarą jest \textit{Maximum Mean Discrepancy}, oryginalnie zaprojektowaną jako test statystyczny do mierzenia różnicy między dwoma rozkładami \textit{bez} założeń o ich kształtach. W tej pracy wykorzystane jest sformułowanie MMD z DrumGAN:
$$
\mathrm{MMD}^2(X,Y)
  = \frac{1}{m(m-1)} \sum_{i \ne j}^{m} k(x_i, x_j)
  + \frac{1}{n(n-1)} \sum_{i \ne j}^{n} k(y_i, y_j)
  - \frac{2}{mn} \sum_{i=1}^{m} \sum_{j=1}^{n} k(x_i, y_j)
$$
gdzie
$$k(a, b) = \frac{16}{16 + \lVert a - b \rVert^2}$$
to funkcja jądra \textit{inverse multi-quadratic kernel} określająca podobieństwo dwóch przykładów (embeddingów), wybrana z powodu dużej czułości na odstające wartości.

\section*{Vendi Score}

Ostatnia metryka pochodzi ze stosunkowo niedawno opublikowanej pracy \textit{The Vendi Score: A Diversity Evaluation Metric for Machine Learning}. Vendi Score jest miarą stricte różnorodności, która
\begin{enumerate}
    \item nie wymaga zbioru referencyjnego, oraz
    \item jest parametryzowana poprzez dowolną funkcję podobieństwa przykładów,
\end{enumerate}
co czyni ją ekspresywną i łatwą w użyciu. Różnorodność, autorzy argumentują, jest inherentną własnością zbioru przykładów i nie powinna zależeć od punktu odniesienia. Vendi Score czerpie z prac w dziedzinie biologii mierzących różnorodność ekologiczną. Definiuje się ją jako wykładnik entropii rozkładu gatunków, co można interpretować jako efektywną liczbę gatunków w populacji - przy jednostajnym rozkładzie po $n$ gatunkach jest ona równa $exp(log(n)) = n$, a jeśli rozkład ten zostanie z całą masą w jednym gatunku będzie ona równa $exp(log(0)) = 1$.

Autorzy Vendi Score przenoszą tę koncepcję w zbiór dowolnych obiektów poprzez macierz podobieństwa. Dla $n$ przykładów i funkcji $k(x, y) \in [0, 1]$ (1 oznacza podobne przykłady, 0 różne) definiują Vendi Score jako
$$VS_k(x_1, \ldots, x_n) = \exp\left(-\sum_{i=1}^{n} \lambda_i \log \lambda_i\right)$$
gdzie $\lambda_i$ to wartości własne macierzy $K/n$, a $K_{ij} = k(x_i, x_j)$. Taką wartość, podobnie jak te z dziedziny biologii, również można interpretować jako efektywną liczbę różnych przykładów w zestawie $X$. W porównaniu z innymi, prostyszymi metrykami różnorodności, Vendi Score ma dwie ważne cechy:
\begin{enumerate}
    \item jeśli $k(x_i, x_j) = 0$ dla każdego $i$, $j$, (i.e. przykłady są maksymalnie różne) to $VS(x_1, \ldots, x_n) = n$, jeśli $k(x_i, x_j) = 1$ dla każdego $i$, $j$, to $VS(x_1, \ldots, x_n) = 1$;
    \item jeśli do przykładów $x_1, \ldots, x_n$ dodamy $x_{n+1}$ taki, że jakiś $k(x_i, x_{n+1}) = 1$, to $VS(x_1, \ldots, x_n) = VS(x_1, \ldots, x_n, x_{n+1})$;
\end{enumerate}
Poprzez serię eksperymentów na prawdziwych i syntetycznych danych, autorzy pokazują, że VS jest w stanie wychwycić więcej detali w testowanych zestawach. Porównując VS z prostą miarą średniego podobieństwa między wszystkimi przykładami pokazują, że VS lepiej reaguje na zmiany w bazowych rozkładach z których losowane są dane syntetyczne. W kwestii prawdziwych zastosowań, autorzy pokazują, że VS da się wykorzystać mierząc różnorodność w GANach w celu wychwycenia \textit{mode collapse} albo przy ewaluacji innych modeli generatywnych.

W przypadku tej pracy użytą miarą podobieństwa jest iloczyn skalarny embeddingów CLAP.