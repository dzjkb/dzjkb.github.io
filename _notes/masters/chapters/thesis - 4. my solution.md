## Raw audio multi-band VAE with a single latent vector
inspired heavily by RAVE/SoundStream - position relative to all the previous works, not only these two
A new paradigm for VAE works - reduce the latent stream to a single vector
- idk if it's appropriate to boast about sth being new
- or about how similar the arch is to RAVE hm
- ok single-latent VAE might be new actually

**the residual convolutional stack**
dimension-preserving blocks of residual dilated convolutions, which is a standard practice atm
symmetrical up/down sampling

**single latent + optional NF posterior**
# NF prior
## DDSP-like noise generator
quick DDSP summary (or put that into related work)
## the discriminator and 2-phase training
krótko - GANy są już opisane w 2 rozdziale
głównie opis descriptowego dyskryminatora, jest co opisywać

---
# text

W tej pracy proponuję model generatywny trenowany na krótkich, 1-2 sekundowych samplach, takich jak na przykład brzmienia perkusyjne. Celem jest, oprócz wysokiej jakości, żeby model udostępnił wysokopoziomowe parametry kontroli nad syntezowanym dźwiękiem, co umożliwiłoby korzystanie z niego jako instrumentu. Zamiast jednak z góry warunkować syntezę na ustalonych parametrach, badam istotne cechy które model sam wyciąga w procesie uczenia. Do tego używam Variational Auto-Encoderów, jako że w przeciwieństwie do GANów architektura ta wymusza tego typu nienadzorowaną ekstrakcję cech (TODO przypis?). Zamiast kodować wejściowy sygnał w ciąg wektorów z przestrzeni ukrytej, tak jak robią to wszystkie prace używające w tym kontekście VAE, kompresuję wejście do pojedyńczego wektora, co pozwala na łatwiejsze modelowanie ukrytej reprezentacji całych sampli oraz pozwala tej reprezentacji na uchwycenie globalnych własności dźwięku. Po wytrenowaniu modeli na trzech zestawach danych, analizuję przestrzeń ukrytą pod kątem struktury i najistotniejszych kierunków.

\section*{Architektura}

Architektura inspirowana jest pracami RAVE oraz SoundStream. Enkoder i dekoder są symetrycznymi sieciami konwolucyjnymi składającymi się głównie z \textit{dylatowanych} i \textit{rezydualnych} bloków. Na schemacie~\ref{fig:vae} widać bloki odpowiednio enkodujące i dekodujące. Enkoder poprzez zestaw takich bloków, gdzie po każdym jest warstwa zmniejszająca częstotliwość sygnału, kompresuje go do istotnie mniejszej ilości kroków czasowych (zwiększając przy tym liczbę kanałów), po czym ostateczna konwolucja tworzy z niego reprezentację w przestrzeni ukrytej - wektor średnich oraz odchyleń standardowych. Dekoder jest odwrotnością tego procesu - wektor z przestrzeni ukrytej jest najpierw rzutowany na krótką sekwencję z dużą ilością kanałów, a potem przepuszczany przez serię bloków między którymi częstotliwość sygnału jest zwiększana poprzez dedykowane warstwy konwolucyjne. Główną asymetrią tych dwóch sieci jest sposób, w jaki dekoder generuje dźwięk. O ile enkoder czyta falę na wejściu zwykłą warstwą konwolucyjną, dekoder używa trzech komponentów do ostatecznego generowania dźwięku.

\begin{figure}[t]
  \centering
  \begin{subfigure}[b]{0.56\textwidth}
    \centering
    \includegraphics{figures/fig_vae_overview.pdf}
    \caption{Cała sieć VAE.}
    \label{fig:vae-overview}
  \end{subfigure}\hfill
  \begin{subfigure}[b]{0.42\textwidth}
    \centering
    \includegraphics{figures/fig_vae_blocks.pdf}
    \caption{Bloki Enkodera i Dekodera.}
    \label{fig:vae-blocks}
  \end{subfigure}
  \caption{Architektura modelu. $B$: liczba pasm PQMF, $C$: parametr capacity,
    $L$: liczba bloków, $D$: wymiarowość przestrzeni ukrytej.}
  \label{fig:vae}
\end{figure}

Końcówka ta składa się z trzech części:
\begin{itemize}
    \setlength\itemsep{0em}
    \item obwiedni amplitudy, która ułatwia rekonstrukcję cichych sygnałów;
    \item generatora samej fali;
    \item komponentu filtrującego szum, gdzie sieć steruje parametrami filtra.
\end{itemize}
Co ważne, na tym etapie sygnał jest wciąż podzielony na pasma i końcówka działa niezależnie dla każdego z nich. Za generowanie fali i obwiednię amplitudy odpowiedzialna jest pojedyńcza warstwa konwolucyjną, która nie zmienia częstotliwości sekwencji a jedynie transformuje wyjście z ostatniego bloku dekodera na wymaganą liczbę kanałów. Obwiednia jest następnie zaaplikowana do fali poprzez zwykłe przemnożenie. Równolegle, sygnał z ostatniego bloku dekodera trafia do generatora szumu, po czym oba sygnały zostają do siebie dodane i ograniczone do dozwolonego zakresu wartości próbek. Schemat~\ref{generator-head} przedstawia całą końcówkę generującą dźwięk. Na samym końcu, za pomocą filtrów PQMF wszystkie pasma zostają zsyntezowane w końcowy dźwięk. Każdy model dzieli wejściowy sygnał na 16 pasm.

\begin{figure}[t]
  \centering
  \includegraphics{figures/fig_generator_head.pdf}
  \caption{Końcówka generująca dźwięk. Wyjście konwolucji jest podzielone na dwie części, $s_1$ and $s_2$;
    $s_2$ moduluje amplitudę $s_1$, a następnie do wyniku dodany jest przefiltrowany szum.}
  \label{fig:generator-head}
\end{figure}

Generator szumu to komponent zapożyczony z pracy DDSP, która eksploruje syntezę dźwięku poprzez sieci granularnie sterujące różnego rodzaju syntezatorami czy efektami. W przeciwieństwie do tradycyjnych algorytmów syntezy, komponenty te są różniczkowalne, co pozwala na wykorzystanie ich jako elementu sieci neuronowej. W pierwszej kolejności, biały szum zostaje podzielony na okna. Następnie, wyjście z dekodera przechodzi przez kilkuwarstwową sieć konwolucyjną przygotowywującą sygnał sterujący do filtra dla każdego z kanałów na wyjściu osobno. W danym oknie filtr jest zdefiniowany poprzez kilka wartości (w tej pracy 6) będącymi docelową charakterystyką amplitudową. Odwrotna transformata Fourier'a zamienia tę charakterystykę na odpowiedź impulsową o zerowej fazie, która następnie jest aplikowana do okna poprzez konwolucję. Zerofazowość gwarantuje, że każda częstotliwość zostanie przesunięta w fazie o tę samą wartość i nie powstaną w tym procesie niepożądane artefakty. Po przefiltrowaniu okna są konkatenowane, dając wyjściowy sygnał.

\section*{NF posterior i prior}

Do rozkładów posterior i prior w VAE opcjonalnie zastosowane są \textit{normalizing flows}. W przypadku rozkładu posterior, NF przekształca wyjście z enkodera, zwiększając przy tym jego ekspresywność i możliwość dopasowania się do regularyzacji. Rozkład prior wciąż zostaje rozkładem standardowym, a NF jest wykorzystany wyłącznie przy treningu lub przy rekonstrukcji. W przypadku rozkładu prior, NF to osobny komponent uczony na reprezentacjach ukrytych przykładów z już wytrenowanej sieci. Prawdziwy rozkład w przestrzeni ukrytej rzadko daje się wyrazić przez rozkład standardowy, więc poprzez zwiększenie ekspresywności i lepsze dopasowanie model jest w stanie potem bezwarunkowo generować bardziej realistyczne przykłady. Schemat~\ref{fig:nf-placement} obrazuje ulokowanie obu komponentów NF w architekturze. Do obu użyte zostały architektury RealNVP.

\begin{figure}[t]
  \centering
  \includegraphics{figures/fig_nf_placement.pdf}
  \caption{Umiejscowienie NF posterior i prior.}
  \label{fig:nf-placement}
\end{figure}

\section*{Trening dwu-fazowy i funkcje straty}

Do treningu użyte są następujące funkcje straty:
\begin{itemize}
    \setlength\itemsep{0em}
    \item wieloskalowa percepcyjna funkcja straty oparta na STFT, podobnie jak w Parallel WaveGAN;
    \item ta sama funkcja straty ale na poszczególnych pasmach, przed połączeniem przez PQMF;
    \item regularyzacja \textit{KLD} z dolnym limitem;
    \item adwersaryjna funkcja straty \textit{hinge loss};
    \item adwersaryjne \textit{feature matching loss}.
\end{itemize}

Podążając za metodą z pracy RAVE, trening podzielony jest na dwie fazy:
\begin{enumerate}
    \setlength\itemsep{0em}
    \item uczenie reprezentacji ukrytej;
    \item dostrajanie dekodera.
\end{enumerate}
W pierwszej fazie uczony jest cały autoenkoder z użyciem percepcyjnej funkcji straty oraz regularyzacji KLD, co jest standardową procedurą trenowania Variational Auto-Encoderów. Ze swojej natury VAE często mają problem z odtwarzaniem szczegółów w swoich rekonstrukcjach. Druga faza treningu ma temu zaradzić poprzez dotrenowanie samego dekodera. W tej fazie enkoder jest zamrożony, zaś dekoder uczony jest z dodatkowymi funkcjami straty typowymi dla GANów - \textit{hinge loss} i \textit{feature matching loss} - opartymi na dwóch sieciach dyskryminujących. W praktyce jest to równoważne GANowej procedurze treningowej, z użyciem wyuczonej reprezentacji ukrytej zamiast wektorów losowego szumu i już pre-trenowanego dekodera. W pierwszej fazie funkcja straty jest ważoną sumą percepcyjnej funkcji straty na całym sygnale, na poszczególnych pasmach, oraz regularyzacji reprezentacji ukrytej. Regularyzacja KLD jest ucięta do minimalnej wartości 0.5, co powoduje, że funkcja straty pozwala modelowi na lekkie odejście od docelowego rozkładu prior bez żadnej penalizacji. W drugiej fazie, do tego dodane są równieżA pozostałe dwa składniki. \textit{Feature matching loss} liczy odległość L1 aktywacji między wszystkimi warstwami sieci dyskryminujących oprócz pierwszej dla przykładu $x$ i jego rekonstrukcji. \textit{Hinge loss} składa się z dwóch części - dla generatora i dla dyskrminatora. Jest zdefiniowane następująco, gdzie $D$ to dyskryminator a $\hat{x}$ rekonstrukcja:
\begin*{align}
    \text{hinge}_D(x, \, \hat{x}) &= \max\left(0, \, 1 - D(x)\right) + \max\left(0, \, 1 + D(\hat{x})\right) \nonumber \\
    \text{hinge}_G(\hat{x}) &= -D(\hat{x})
\end{align}

\section*{Sieci dyskryminujące}

Sieci dyskryminujace do drugiej fazy treningu są pożyczone z pracy RVQGAN. Stosuje ona dwa rodzaje sieci:
\begin{enumerate}
    \setlength\itemsep{0em}
    \item \textit{Multi-scale discriminator} (MSD) - oparta na podobnej zasadzie co percepcyjna funkcja straty;
    \item \textit{Multi-period discriminator} (MPD) - przekształcająca wejście w taki sposób, by łatwiej wychwytywać okresowość sygnału na różnych skalach.
\end{enumerate}

W przypadku pierwszej sieci, w przeciwieństwie do percepcyjnej funkcji straty, chcemy jednak na tym etapie penalizować źle zrekonstruowaną fazę sygnału. Ponieważ spektrogramy mocy zatracają tę informację, dodatkowo używa zespolonych komponentów zwracanych przez STFT. Po przekształceniu sygnału jest on dzielony na 5 (nierównych) pasm, przetworzony przez oddzielną 5-warstwową dwu-wymiarową sieć konwolucyjną dla każdego pasma i na końcu ostatnią warstwę zbierającą wszystko w wynik.

Druga sieć działa bezpośrednio na ciągu próbek, składając się z oddzielnej instancji dla każdego okresu z listy $p \in \lbrace 2, 3, 5, 7, 11 \rbrace$. W danej podsieci MPD z okresem $p$, jedno-wymiarowy ciąg wejściowy o długości $T$ jest najpierw przekształcany w dwu-wymiarowy o wymiarach $(T/p, \; p)$, na którym następnie dwu-wymiarowa sieć konwolucyjna liczy ostateczny wynik. Filtry w warstwach konwolucyjnych mają rozmiary \texttt{(n, 1)}, przez co biorą pod uwagę tylko jeden wiersz z wejścia naraz, czyli \texttt{n} kolejnych próbek odległych o $p$. Taka transformacja pozwala jej łatwiej wychwytywać niezgodności w okresowości fali na różnych skalach. Schemat~\ref{fig:discriminators} przedstawia działanie MSD i MPD.

\begin{figure}[t]
  \centering
  \begin{subfigure}[b]{0.56\textwidth}
    \centering
    \includegraphics{figures/fig_disc_msd.pdf}
    \caption{Dyskryminator MSD.}
    \label{fig:disc-msd}
  \end{subfigure}\hfill
  \begin{subfigure}[b]{0.42\textwidth}
    \centering
    \includegraphics{figures/fig_disc_mpd.pdf}
    \caption{Dyskryminator MPD.}
    \label{fig:disc-mpd}
  \end{subfigure}
  \caption{Sieci dyskryminujące. MSD jest instancjonowany dla wielkości okna 2048, 1024, 512; MPD dla okresów 2, 3, 5, 7, 11.}
  \label{fig:discriminators}
\end{figure}