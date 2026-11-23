threads after wavenet:
- pretty much everything is convolutions
- voice vs music vs codecs
- GANs vs VAEs vs diffusion and everything in between
- real-time
- multimodality - audio + LMs
- music: creative usage vs entire track generation
year after year? maybe the first couple of years, then split into the different threads
describe in more detail:
- WaveNet -> RAVE
- DrumGAN -> StyleGAN
#### czym to nie jest?
muzyka w innych reprezentacjach - np midi
music intelligence - klasyfikacja czy wyciąganie innych informacji o dźwięku
#### first steps
Pierwszą pracą która podjęła się bezpośredniego modelowania dźwięku za pomocą sieci neuronowych to WaveNet z 2016 roku. Celem były systemy TTS, co też ma swoje odzwierciedlenie w ewaluacjach, jednak autorzy zawarli również mały jakościowy eksperyment pokazujący potencjał ich rozwiązania dla muzyki.

// opis wavenetu:
- konwolucje - dilated causal i tyle
- zdecydowanie lepsze wyniki niż tradycyjne systemy - obiecujący kierunek

Niedługo po WaveNet, ludzie z Google Brain zbadali jego stricte muzyczne wykorzystanie - generowanie monofonicznych ścieżek z wybranych instrumentów. W swojej pracy "Neural audio synethesis with WaveNet auto-encoders", ...

// opis NSynth:
- zestaw danych - one shot instrumenciki
- architektura - wavenet encoder/decoder, residual warstwy

#### TTS (commercial) vs. muzyka (creative)
Tacotron(2), SING - synteza nastawiona na TTS vs. synteza muzyki
#### GANy
Kolejne prace zaczęły eksplorować wykorzystanie GANów do tego zadania. GANy naturalnie wpisują się w ten obszar z dwóch powodów:
- są generatywne
- są stworzone do generowania przykładów o wysokiej jakości
- autoregresywne modele jak WaveNet są bardzo wolne
Jako, że GANy były oryginalnie stosowane do obrazu, prace jak WaveGAN czy GANSynth próbowały generować nimi spektrogramy, które następnie można przekonwertować do audio za pomocą dostępnych narzędzi. MelGAN jako pierwsza praca postawiła sobie za cel zbadać to, na ile GANy mogą bezpośrednio generować dźwięk. Istotnym elementem tych prac są dyskryminatory - nie są to po prostu kolejne warstwy konwolucyjne, a sieci specjalnie skonstruowane tak żeby eksploatować własności dźwięku i nakierowywać generator na własności które wytworzą wysokiej jakości brzmienie.
#### realtime
Kolejnym istotnym elementem wykorzystanym w Multi-band MelGAN, kontynuacji MelGAN, jest najpierw rozkład przetwarzanego sygnału na pasma.

// PQMF -> realtime
Parallel WaveGAN, DurIAN do this too
realtime args?
#### VAE, codeci (commercial)
RAVE, SoundStream/EnCodec/RVQGAN
and later on combinations with diffusion
#### diffusion (creative)
Crash, DRAGON (deez nuts?)
combinations with VAEs for representation learning
#### whole-track generation, multimodality (commercial)
Jukebox -> ... -> musicLM, Suno etc
#### putting it all together (modern TTS)
VALL-E (2)? ElevenLabs v4 - closed source :c

---
# pre-text
>Innym sposobem są sieci neuronowe. W 2016 roku, w przełomowej pracy WaveNet \cite{wavenet}, autorzy podjęli się zadania generowania dźwięku bezpośrednio za pomocą jednowymiarowych sieci konwolucyjnych. Głównym ich celem była synteza mowy, lecz pokazali obiecujące wyniki również w zakresie generowania muzyki. Zastosowanie \textit{dilated} i \textit{causal} konwolucji, inspirowane wcześniejszymi podejściami do generowania obrazu piksel po pikselu, pozwoliło na bezpośrednie modelowanie surowego sygnału audio mimo ogromnej wymiarowości - 16000 próbek na sekundę. WaveNet, jako system TTS (text to speech), brał na wejśiu tekst, co oznacza, że już zawierał sposoby kontroli procesu syntezy. Nie były one jednak wykorzystane w przypadku eksperymentów z syntezą muzycznych brzmień.

>WaveNet położył fundament pod kolejne 10 lat badań nad zastosowaniem sieci neuronowych bezpośrednio do syntezy dźwięków różnego rodzaju. W następnych latach prace nad dźwiękiem szły dwoma głównymi torami - jednym z nich jest synteza mowy, drugim jest synteza muzyki.

how deep into each work do I want to go?
- not into architecture details like parameters
- datasets - no
- general conceptual improvements
- aims/contributions of each paper
- sth like 1 short paragraph per paper? or per concept, sometimes grouping several together (one paper can be in multiple groups)
ok ok ok
- **wavenet** - pierwsza praca generująca bezpośrednio dźwięk, głównie mowa troche muzyka, dobra jakość duża wymiarowość autoregresywność powolne ale gg fundamentalna praca
- generowanie spektrogramów - **tacotron 1**/**tacotron 2**
- (?) również próba RNNów - **SampleRNN**, ale nie poszło daleko
    - albo połączyć **WaveGlow** i RNNy w "inne metody - RNNy, flows" które nie poszły daleko
- pierwsze muzyczne zastosowania - **NSynth**, **SING**
- GANy - **WaveGAN** (fala), **GANSynth** (spektr), **MelGAN** (spektr + dekoder), 
- ~~(?) TTS - MelGAN, WaveGAN~~
- w strone real-time - **multiband MelGAN** <- (**Durian** (brief mention), pwg, melgan), **Parallel WaveGAN** <- **ClariNet** <- **Parallel WaveNet** (also brief mentions)
    - fuck but SING or GANSynth already had real-time speeds, so who cares about these
    - może nie "w strone real-time" ale po prostu wątek pt. przyśpieszanie?
    - bardzo krótko o podejściach typu destylacja, a potem - PQMF
- VAEs - **Neural Drum Machine** (music, intro VAE, used because generative), **Jukebox** (całe tracki), **RAVE** (same - but in a musical, not codec context)
- VAEs for codecs - **SoundStream** (mention some of the previous works maybe or combine with EnCodec or RVQGAN), **EnCodec**, **RVQGAN** (based on HiFi-GAN and BigVGAN)
- music (Sony CSL w strone drum samplerka) (albo samplerki ale że w strone kontrolowalności?) (w kierunku instrumentów muzycznych i ich kontroli) - **Neural Percussive Synthesis** (<- NDM), **DrumGAN**, **DrumGAN VST** (productionized), **Crash** (intro diffusion), maybe DRAGON? maybe style-drumsynth?
- generally - summing up latest threads? I can't hope to describe things like CLAP or CALM in enough detail, and really have no reason to (in this chapter at least)
    - (?) w strone multi-modality - **CLAP**, **MusicLM**
    - (?) obecnie ogólnie - **DRAGON**, **CALM**
        - DRAGON adds vendi regularization, idk
    - (?) obecnie kreatywne-muzyczne - **Platune**
    - (?) obecnie industry - closed source od **Suno**, **ElevenLabs**

---
# tekst
\section*{Bezpośrednia synteza dźwięku sieciami neuronowymi}

Pierwszą pracą, która podjęła się bezpośredniej syntezy dźwięku za pomocą sieci neuronowych jest WaveNet z 2016 roku. Dźwięk jest wyjątkowo skomplikowanym sygnałem na który składa się wiele zależności rozłożonych po wielu skalach czasowych, od kilku milisekund, sekund, czy minut, do potencjalnie dowolnej długości. W swojej cyfrowej reprezentacji zapisany dźwięk zwykle składa się z 44100 lub 48000 próbek na sekundę, co oznacza że dla sieci neuronowej modelowanie jednej sekundy wymaga nauczenia się zależności między wieloma tysiącami różnych wymiarów. WaveNet ograniczył się do częstotliwości próbkowania 16kHz, co wciąż jest wystarczające do generowania zrozumiałej mowy a znacząco ogranicza złożoność problemu. Praca PixelCNN z tego samego roku rozwiązała podobny problem w dziedzinie obrazu autoregresywnie modelując zdjęcie piksel po pikselu. Mimo dużej złożoności - wielu tysięcy pikseli - rozwiązanie to pokazało obiecujące wyniki w generowaniu spójnych przykładów. Wykorzystując podobną architekturę opartą na konwolucjach, autorzy WaveNet pokazali, że modelując dźwięk próbka po próbce da się osiągnąć lepszą jakość dźwięku niż inne, bardziej klasyczne metody.

Głównym elementem architektury WaveNet pozwalającym na modelowanie wielu tysięcy próbek naraz są "rozrzedzone" (ang. \textit{dilated}) konwolucje. Oprócz tego, żeby zachować zależności czasowe - żeby próbka N zależała wyłącznie od wcześniejszych próbek N-1, N-2, - konwolucje te są również "przyczynowe". Na obrazku 1 widać schematyczne przedstawienie przyczynowych rozrzedzonych warstw konwolucyjnych. Przy takiej architekturze rozmiar wejścia sieci rośnie wykładniczo z liczbą warstw, umożliwiając modelowanie tysięcy próbek naraz zachowując obliczeniowo praktyczny rozmiar sieci. W przypadku WaveNet pojedyńcza próbka na wyjściu zależy od ~300 milisekund na wejściu, co przy 16000 próbkach na sekundę oznacza ~5000 próbek. Cała sieć składała się z kilku bloków takich konwolucji, które miały również połączenia \textit{rezydualne} (przypis) oraz \textit{skip connections} (przypis), co również przyczyzniło się do możliwości wyuczenia tak głębokiej sieci.

WaveNet skupił się przede wszystkim na kwestii syntezy mowy. Dotychczasowe rozwiązania dzieliły się na dwa rodzaje - \textit{konkatenacyjne} i \textit{parametryczne}. (TODO refs?) Pierwsze syntezuje mowę poprzez łączenie fonemów wyciętych z prawdziwych nagrań w pożądany tekst, zaś drugie uczy się statystycznych modeli (na przykład łańcuchów markowa) do generowania parametrów \textit{wokoderów} - syntezatorów tworzonych specjalnie pod imitację głosu. Generowanie mowy musi być oczywiście warunkowana na tekście, który chcemy usłyszeć. WaveNet jako wejście do sieci wykorzystuje ciąg \textit{fonemów} (przypis) zamiast tekstu, czyli reprezentacji bardziej zbliżonej do rzeczywistego brzmienia mowy. Pozwala to ograniczyć zadanie sieci do generowania samego brzmienia zamiast uczenia się zasad wymowy, co pomogło osiągnąć dobrą jakość dźwięku dla niewielkiego jednak pola receptywnego ~300 milisekund (odpowiadającegoi mniej więcej 2-3 fonemom). Autorzy do ewaluacji swojego podejścia użyli subiektywnego testu preferencji oraz \textit{Mean Opinion Score} dla angielskiego oraz chińskiego. W teście preferencji zestaw ludzkich ewaluatorów musi wybierać która z dwóch przedstawionych próbek dźwięku brzmi bardziej naturalnie, albo zaznaczyć brak preferencji. W \textit{Mean Opinion Score} każdy ewaluator ocenia ileś losowych próbek w skali od 1 do 5, a wynikiem jest średnia ocen dla danego modelu. W obu metrykach WaveNet pokazał znaczącą przewagę nad poprzednimi podejściami, pokazując potencjał sieci neuronowych w zadaniu bezpośredniego generowania dźwięku.

Jako poboczny eksperyment, autorzy WaveNet wytrenowali bezwarunkowy (tzn. bez warunkującego sygnału takiego jak fonemy) model na dwóch muzycznych zestawach danych. Nie raportowali dla nich ilościowych metryk jakości, tylko udostępnili przykłady generacji i krótką subiektywną, jakościową ewaluacje. Przykłady te pokazują, że WaveNet jest również w stanie nauczyć się reprodukować brzmienie muzycznych instrumentów w satysfakcjonującej jakości, zachowując spójność w krótkiej skali odpowiadającej długości pola receptywnego.

\section*{Generowanie dźwięku w domenie częstotliwości}

Równolegle eksplorowaną alternatywą do generownia dźwięku w domenie czasu - próbka po próbce - było generowanie go w domenie częstotliwości, czyli generowanie \textit{spektrogramów}. Ponieważ dowolny słyszalny sygnał dźwiękowy da się rozłożyć na sumę indywidualnych okresowych sygnałów (fal sinusoidalnych) w zakresie 20-20000Hz, możemy go reprezentować w postaci wykresu zawartości poszczególnych tych składowych. Analiza sygnału pod względem okresowości wymaga odczytów tego sygnału w jakimś zakresie czasu - okres ten jest hiperparametrem algorytmów robiących taką analizę (przypis transformata fouriera). Jeśli sygnał jest dłuższy niż ten okres, możemy zaaplikować taką analizę do kolejnych fragmentów - "okien" - sygnału, dostając w wyniku zestaw spektrogramów w czasie odpowiadające wejściowemu dźwiękowi. Pierwszą pracą która badała bezpośrednie generowanie spektrogramów sieciami neuronowymi w kontekście systemów TTS to Tacotron z 2017 roku. W porównaniu z WaveNet, autorzy wskazują na większy charakter end-to-end ich rozwiązania, czyli działanie bezpośrednio na tekście, a nie na fonemach które w jakiś sposób trzeba najpierw wydobyć. Tacotron bazuje na poprzednich parametrycznych podejściach, które stosowały sieci rekurencyjne w wybranych etapach systemu do generowania parametrów dla wokodera. Zastępuje wokoder poprzez moduł złożony z konwolucji i dwukierunkowej sieci rekurencyjnej, generując kolejne "okna" spektrogramu w czasie, a na koniec używa standardowego algorytmu Griffin-Lim do rekonstrkcji fazy wraz z odwrotną transformatą Fourier'a żeby zsyntezować z nich falę dźwiękową w postaci próbek. Algorytm ten jednak nie jest idealny, a natura spektrogramów powoduje, że w zależności od długości okna albo tracimy dokładność odczytu składowych częstotliwości (krótkie okno) albo tracimy szczegóły czasowe w generowanym sygnale (długie okno).

Tacotron 2 z kolejnego roku adresuje te niedociągnięcia łączac spektrogramy z siecią typu WaveNet. Podobnie jak w pierwszej wersji tekst jest transformowany w ciąg spektrogramów, jednak do wygenerowania samej końcowej fali użyta jest zmodyfikowana wersja WaveNet, z ciągiem spektrogramów jako sygnał warunkujący. Rodzaj użytych spektrogramów jest również inny, ze skalą częstotliwości Mel (przypis) zamiast liniowej. Zachowują one więcej detali w niskich częstotliwościach, co jest istotne dla zrozumienia mowy, ale dają gorsze wyniki przy użyciu algorytmu Griffin-Lim. Jako że spektrogram jest reprezentacją bliższą rzeczywistej fali dźwiękowej niż fonemy, użyty WaveNet jest mniejszy niż z oryginalnej pracy. Wyniki pokazują znaczną poprawę jakości względem pierwszej wersji, a test preferencji względem oryginalnych klipów pokazuje, że to rozwiązanie jest już znacząco zbliżone do prawdziwej, nagranej mowy.

\section*{Zastosowania muzyczne}

Niedługo po opublikowaniu WaveNet, badacze zaczęli eksplorować sieci do generowania dźwięku w stricte muzycznych kontekstach. Jedną z pierwszych takich prac, z 2017 roku, był NSynth, który do zadania generowania pojedyńczych nut z różnych instrumentów zastosował dwie sieci WaveNet złączone w swojego rodzaju autoencoder, kompresujący każdy przykład w ciąg 125 16-wymiarowych wektorów Z. Dekoder następnie, podobnie do oryginalnego WaveNet, autoregresywnie generował dźwięk będąc warunkowanym na wektorach Z. Częścią tej pracy było stworzenie takiego zestawu danych o wysokiej jakości i niewielkiej liczbie osi zróżnicowania poszeczególnych przykładów, pozwalającego następnie dokładnie zbadać semantykę przestrzeni ukrytej. Jest to:
\begin{itemize}
    \setlength\itemsep{0em}
    \item 306 043 przykładów z częstotliwością próbkowania 16kHz;
    \item każdy przykład trwa 4 sekundy - przez 3 sekundy nuta jest trzymana, 1 sekundę wybrzmiewa po puszczeniu;
    \item każda nuta jest wygenerowana przez jeden z z 1006 instrumentów cyfrowych w jednej z 5 głośności i w jednej z 88 wysokości (standardowy zakres fortepianu), co oznacza, że ma konkretnie zdefiniowane i stałe brzmienie/wysokość.
\end{itemize}
Autorzy teoretyzowali, że przy takiej strukturze wektory Z powinny w niezależny sposób kodować wysokość, barwę oraz głośność dźwięku. Do ewaluacji hipotezy użyli metryki a la Inception Score, mierzącej jak wyraźnie przykład należy do jednej z kilku klas (co mogło być inspiracją dla WaveGAN), ale też \textit{sondowania liniowego} (ang. \textit{linear probing}) - precyzji liniowych klasyfikatorów próbujących zaklasyfikować wysokość dźwięku przykładu bazując \textbf{tylko na jego reprezentacji Z}. Pierwsza z tych metryk pośrednio mierzy jakość wygenerowanego dźwięku, zaś druga mierzy to, czy informacja o wysokości dźwięku rzeczywiście została niezależnie zakodowana w przestrzeni ukrytej. Porównując się z własnoręcznie stworzoną metodą referencyjną - autoencoderem generującym spektrogramy wraz z algorytmem Griffin-Lim, rekonstruującym falę za pomocą estymacji fazy - Autorzy NSynth pokazali, że autoencoder bazujący na WaveNet jest w stanie w nienadzorowany sposób nauczyć się muzycznie istotnych cech.

Praca SING z kolejnego roku, korzystając z zestawu NSynth, podjęła się zadania stworzenia czegoś już bardziej przypominającego syntezator - sieci generującej dźwięk na podstawie metadanych identyfikujących pożądany instrument, wysokość oraz głośność dźwięku, czyli na podstawie trzech osi wariacji które NSynth dostarcza. Autorzy zauważyli, że autoregresywność to silne ograniczenie dla praktycznych zastosowań takiej sieci i przeformułowali dekoder tak, żeby się jej pozbyć. Żeby móc wyjść od wcześniej wspomnianych metadanych, dodali do całego systemu kolejny komponent - sieć rekurencyjną LSTM warunkowaną na wysokości/barwie/głośności dźwięku, która uczy się rekonstruować ciąg wektorów z przestrzeni ukrytej. W końcowej formie sieć SING generuje fale dźwiękową w następujących krokach:
\begin{itemize}
    \setlength\itemsep{0em}
    \item na wejściu dostaje wartość identyfikującą głośność, instrument oraz wysokość dźwięku;
    \item wartości te są przekonwertowane w wektory liczb rzeczywistych za pomocą wyuczonych \textit{lookup tables};
    \item sieć LSTM, dostając na każdym kroku krotkę (głośność, instrument, wysokość, krok czasu), gdzie krok czasu to po prostu kolejne liczby od 1 do 265, generuje ciąg 128 wymiarowych wektorów z przestrzeni ukrytej;
    \item dekoder oparty na konwolucjach z tego ciągu wektorów generuje falę dźwiękową na poziomie próbek.
\end{itemize}
Za pomocą \textit{Mean Opinion Score} autorzy pokazali, że takie rozwiązanie jest w stanie rekonstruować przykłady w znacząco lepszej jakości niż takie oparte na WaveNet. Do tego, sama wytrenowana sieć jest prawie 4 razy lżejsza, trening trwa około 30 razy krócej a dźwięk jest generowany około 2500 razy szybciej.
Wskazują oni na dwa rozwiązania, które im to umożliwiły: 
\begin{itemize}
    \setlength\itemsep{0em}
    \item autorska, "percepcyjna" funkcja straty oparta na spektrogramach, oraz
    \item użycie dwu-fazowego treningu - najpierw dekoder z pomocniczym, konwolucyjnym enkoderem, a potem dotrenowanie sieci LSTM i dekodera bazując na wyjściach tego enkodera.
\end{itemize}
"Percepcyjna" funkcja straty jest zdefiniowana jako odległość między spektrogramami mocy. Uwzględnia ona w ten sposób pewną ważną cechę ludzkiego postrzegania dźwięku - w dużej mierze jest ono niezależne od fazy danej fali. Weźmy na przykład falę sinusoidalną o częstotliwości 440Hz z częstotliwością próbkowania 16kHz, gdzie i-ta próbka jest dana przez: $$x_i = sin(2\pi i \frac{440}{16000} + \phi)$$a $\phi$ oznacza początkową fazę. Zarówno dla $\phi = 0$ jak i $\phi = \pi$, taka fala będzie brzmiała tak samo, jednak naiwnie zdefiniowana funkcja straty typu \textit{Mean Squared Error} - $\frac{1}{N}\sum (x_i - y_i)^2$ - będzie duża. Spektrogram mocy (TODO czy definicja) odrzuca informacje o fazie, przez co dwie takie fale różniące się tylko $\phi$ dadzą ten sam wynik, co z kolei oznacza odległość równą 0. Autorzy porównują oba podejścia, pokazując, że o ile trening oparty na MSE wystarczy do sprawnie działającego autoencodera, kombinacja LSTM + dekoder wymaga tej nowej funkcji straty żeby osiągnąć satysfakcjonujące wyniki.

\section*{Sieci Adwersaryjne}

Dotychczasowo omawiane metody polegały albo na \textit{autoregresywności}, albo sieciach rekurencyjnych w celu generowania nowych przykładów. Równolegle rozwijał się inny, obiecujący sposób generatywnego modelowania - GANy. Na początku GANy pokazywały wyjątkowe dobre, w porównaniu z innymi metodami, wyniki w kwestii generowania obrazu, co przyczyniło się do dużej ilości badań z ich użyciem. Pierwszą pracą która spróbowała zaaplikować GANy do problemu syntezy dźwięku był WaveGAN z 2018 roku, jednak w innym trybie niż wcześniej omawiane prace. Zamiast generowania na przykład mowy na podstawie sygnału warunkującego - tekstu - WaveGAN podejmuje się probelmu \textit{bezwarunkowego} generowania przykładów podobnych do danych treningowych. Sieć generująca bazuje na \textit{transponowanych} jednowymiarowych konwolucjach, jako że wektor szumu jest z niskowymiarowej przestrzeni i musi ona zwiększać wymiarowość swojego wejścia. Z powodu bezwarunkowego paradygmatu modelowania, ciężko porównywać dwie metody poprzez np. test preferencji rekonstrukcji danego zdania jak w przypadku WaveNet. Autorzy zastosowali zamiast tego metryki jakości i różnorodności stworzone stricte pod generatywne modele:
\begin{itemize}
    \setlength\itemsep{0em}
    \item Inception Score, mające mierzyć jakość, oraz
    \item średnia odległość przykładu od najbliższego sąsiada, mająca mierzyć różnorodność generacji.
\end{itemize}
Na danych składających się z pojedyńczych słów - liczb od 0 do 9 - pokazały one, że WaveGAN jest w stanie generować zrozumiałe i różnorodne przykłady. Nie był on jednak porównany z wyżej omawianymi pracami.

Autorzy NSynth również próbowali zaadresować ograniczenia autoregresywnych modeli w swojej pracy GANSynth. Zamiast jednak bezpośrednio generować falę, ich rozwiązanie generuje ciąg spektrogramów w czasie, podobnie do systemu Tacotron. Autorzy wskazują na dwie rzeczy, które pozwalają im osiągnąć dobrą jakość generowanego dźwięku:
\begin{itemize}
    \setlength\itemsep{0em}
    \item użycie skali Mel w połączeniu z dużym oknem dla STFT, co pozwala zwiększyć precyzję spektrogramów w niskich częstotliwościach, oraz
    \item dodatkowe generowanie fazy sygnału w postaci \textit{instantaneous angular frequency} (TODO przypis).
\end{itemize}
Dodatkowa informacja o fazie oznacza, że konwersja do fali dźwiękowej nie wymaga użycia algorytmu takiego jak Griffin-Lim lub wyuczonej sieci dekodującej jak w Tacotron 2 lub SING, lecz może być standardowo zrobiona poprzez odwrotną transformatę Fourier'a. Do ewaluacji rozwiązania używają metryk inspirowanych poprzednimi pracami - test preferencyjny, Inception score, oraz klasyfikator wysokości dźwięku - jak i dwóch nowych w dziedzinie audio: 
\begin{itemize}
    \setlength\itemsep{0em}
    \item metryka różnorodności \textit{Number of Statistically-Different Bins}. Przykłady treningowe są najpierw klastrowane w 50 klastrów przez k-means (przypis) (w przestrzeni spektrogramów), a następnie wygenerowane przykłady są przypisane do najbliższego klastra. NBD równa się liczbie klastrów w których liczba treningowych i wygenerowanych przykładów różni się w statystycznie istotny sposób, zgodnie z testem dwumianowym.
    \item metryka jakości i różnorodności \textit{Frechet Inception Distance}, zaadaptowana z dziedziny generowania obrazu. Embeddingi dla przykładów są wyciągane z aktywacji klasyfikatora wysokości; szczegóły tej metody zostaną omówione w późniejszych rozdziałach.
\end{itemize}
ok wyniki wyniki i co osiągnęli

WaveGAN i GANSynth pokazały, że GANy mogą znaleźć zastosowanie również w syntezie dźwięku. Praca MelGAN spróbowała zaaplikować je do syntezy mowy, porównując się między innymi z pracami takimi jak Tacotron 2 czy WaveNet. W tym celu, zamiast typowego dla GANów zadania bezwarunkowej generacji, autorzy ograniczyli się do kroku syntezy dźwięku na bazie ciągu spektrogramów, stosując do tego schemat generator + dyskryminator. Spektrogramy uzyskali na dwa sposoby, zależnie od przeprowadzonego eksperymentu:
\begin{itemize}
    \setlength\itemsep{0em}
    \item Dla ewaluacji samej inwersji spektrogramów - standardowo z nagranego dźwięku poprzez transformatę Fourier'a.
    \item Dla ewaluacji MelGAN jako części systemu TTS - używając sieci Text2Mel, również bazującej na konwolucjach.
\end{itemize}
Ich eksperymenty nie pokazały, że takie podejście znacząco przebija jakość dźwięku poprzedników, jednak autorzy wskazują na dwie główne kontrybucje: prędkość oraz porównywalna jakość dźwięku stosując wyłącznie adwersaryjne funkcje straty, nie specjalnie zaprojektowane funkcje mające przybliżać ludzką percepcję dźwięku. W porównaniu z typową funkcją straty opartą na negative log-likelihood kolejnych próbek w oryginalnym WaveNet, adwersaryjne funkcje - czyli próba maksymalizacji wyjścia dyskryminatora oraz różnica aktywacji jego pośrednich warstw (tzw. \textit{feature matching loss}) - są wskazywane tutaj jako bardziej zbliżone do ludzkiej percepcji. Jest tak dlatego, że dyskryminator uczy się rozróżniać prawdziwe od syntetycznych przykładów na bazie charakterystyki brzmieniowej, więc celem sieci staje się nie dokładna rekonstrukcja próbka po próbce, lecz rekonstrukcja zbliżona brzmieniowo. Oprócz tych dwóch eksperymentów, żeby pokazać, że MelGAN można zastosować do innych pośrednich reprezentacji dźwięku niż spektrogram, autorzy przeprowadzili eksperymenty z podstawieniem MelGAN w miejsce dekoderów w dwóch pracach w okół autoenkoderów - Universal Music Translation Network oraz VQ-VAE. Obie te prace do samej generacji fali dźwiękowej używały autoregresywnej sieci typu WaveNet działającej na wyuczonej, ukrytej reprezentacji. Jej podmiana na adwersaryjnie uczoną sieć typu MelGAN wykazała podobną jakość dźwięku przy wielokrotnie zwiększonej przepustowości - nawet 2500 szybciej niż oryginalna praca.

\section*{Inne metody}

W międzyczasie, kilka innych architektur sieci było róœnież badanych pod kątem generowania dźwięku.

\section*{Przyśpieszanie syntezy}

Równolegle do prac eksplorujących inne architektury czy zastosowania, niektórzy badacze skupiali się na samej prędkości syntezy dźwięku. Mimo, że już na przykład podejścia oparte o GANy były w stanie osiągnąć synteze w czasie rzeczywistym, dalsze usprawnienia w tym obszarze były ważnym elementem do praktycznych zastosowań modeli. Autorzy WaveNet, krótko po oryginalnej publikacji, próbowali to zrobić w Parallel WaveNet poprzez destylację sieci do mniejszej, nie-autoregresywnej opartej na \textit{normalizing flows} (NF są opisane bardziej szczegółowo w następnym rozdziale). ClariNet z kolejnego roku kontynuował ten pomysł, dodając małe ulepszenia zwiększające jakość dźwięku. Parallel WaveGAN z początku 2020 roku odszedł od pomysłu destylacji, biorąc za to inspirację z adwersaryjnych podejść. Autorzy użyli nie-autoregresywnej wersji WaveNet jako dekodera do ciągu wektorów szumu, efektynwie zrównoleglając WaveGAN. Mimo tego, że \textit{Mean Opinion Score} nie pokazał przewagi nad ClariNet, autorzy wskazują na prawie 5 razy szybszy trening i 2 razy mniejszy model przy prawie równej jakości dźwięku jako najistotniejsze osiągnięcia tej pracy. Oprócz ewaluacji bezwarunkowej generacji za pomocą \textit{Mean Opinion Score}, przeprowadzili również eksperymenty pod kątem TTS, dodając do sieci sygnał warunkujący w postaci ciągu spektrogramów. Uzyskali w ten sposób sieć bardzo przypominającą MelGAN w zastosowaniu, jednak różniącą się w kluczowych architekturalnych kwestiach. Jedną z ważniejszych, nowatorskich rzeczy zaproponowanych przez Parallel WaveGAN była \textit{wielo-skalowa} percepcyjna funkcja straty oparta na spektrogramach. Rozwiązuje ona ograniczenia spektrogramów związane z wyborem długości okna, które albo zatraca informacje czasowe w sygnale albo dokładność odczytu poszczególnych pasm częstotliwości, poprzez sumowanie kilku funkcji strat na tym samym sygnale o różnych długościach okna. Eksperymenty pokazały, że zarówno dla ClariNet jak i dla Parallel WaveGAN użycie takiej funkcji straty versus zwykłej jest kluczowe dla osiągnięcia wysokiej jakości dźwięku.

Multi-band MelGAN z drugiej połowy 2020 roku zrobił kolejny istotny krok w stronę przyśpieszonej syntezy dźwięku. Bazując na architekturze MelGAN i używając wielo-skalowej funkcję straty od Parallel WaveGAN, autorzy wytrenowali sieć niewiele większą niż Parallel WaveGAN i 7 razy szybszą niż oryginalny MelGAN, osiągając przy tym równą lub lepszą jakość dźwięku w zależności od eksperymentu. Kluczową do tego zmianą było operowanie na dźwięku rozdzielonym na pasma, zainspirowaną przez podobne rozwiązanie w pracy DurIAN badającą jednoczesną syntezę mowy i sztucznych wyrazów twarzy. Pasma te następnie wchodzą do pojedyńczej sieci, która zarówno na wejściu jak i na wyjściu ma odpowiednio dostosowaną liczbę kanałów. Dekompozycja sygnału dźwiękowego o częstotliwości próbkowania S na N pasm zmniejsza częstotliwość każdego pasma do S/N, co przyśpiesza generację dźwięku na dwa sposoby:
\begin{itemize}
    \setlength\itemsep{0em}
    \item pozwala na zmniejszenie liczby kanałów wewnętrznych warstw konwolucyjnych, oraz
    \item zmniejsza rozmiar sekwencji na której operują warstwy wewnętrzne 4-krotnie.
\end{itemize}
Do rozdzielenia na wiele pasm i ich syntezy z powrotem w jeden sygnał użyto zestawu filtrów PQMF (które są opisane w następnym rozdziale). Dzięki takim zmianom architekturalnym Multi-band MelGAN umożliwił badania w stronę znacznego wzrostu złożoności modeli, zachowując przy tym ich potencjał do praktycznych zastosowań.

\section*{Variational Auto-Encodery}

W podobnym czasie, w którym powstawał MelGAN, z laboratorium Sony wyszła praca Neural Drum Machine we współpracy z IRCAM w Paryżu. Tak jak MelGAN, używa spektrogramów mel jako pośredniej reprezentacji dźwięku, z której następnie sieć konwolucyjna generuje falę dźwiękową. Główną jej częścią jest jednak Wasserstein Auto-Encoder uczący swanie wejściowego dźwięku w nisko-wymiarowej przestrzeni ukrytej. Umożliwia to dwie rzeczy, których żadne z poprzednich podejść nie dawało jednocześnie:
\begin{itemize}
    \setlength\itemsep{0em}
    \item bezwarunkowe generowanie nowych przykładów, oraz
    \item wysoko-poziomową kontrolę nad generowanymi przykładami.
\end{itemize}
Dodatkowo NDM dostaje wejście warunkujące będące na przykład kategorią instrumentu, co pozwala na kolejny poziom kontroli. Generowanie nowych pię rekonstruować spektrogramy, tworząc w tym procesie kodowanie wejściowego dźwięku w nisko-wymiarowej przestrzeni ukrytej. Umożliwia to dwie rzeczy, których żadne z poprzednich podejść nie dawało jednocześnie:
\begin{itemize}
    \setlength\itemsep{0em}
    \item bezwarunkowe generowanie nowych przykładów, oraz
    \item wysoko-poziomową kontrolę nad generowanymi przykładami.
\end{itemize}
Dodatkowo NDM dostaje wejście warunkujące będące na przykład kategorią instrumentu, co pozwala na kolejny poziom kontroli. Generowanie nowych przykładów odbywa się poprzez standardowy mechanizm dla VAE, czyli wylosowanie wektora z przestrzeni ukrytej z rozkładu normalnego. Autorzy zaznaczają przy tym, że w praktyce struktura przestrzeni ukrytej odbiega od tego rozkładu na tyle, że tak wygenerowane przykłady są troche gorszej jakości niż rekonstrukcje. Ważnym elementem tej pracy, który umotywował taki wybór architektury, jest implementacja wtyczki VST w systemie Max4Live pozwalającej na syntezę dźwięków za pomocą wytrenowanej sieci od razu w środowisku produkcyjnym, gdzie mogę być one dalej użyte w kompozycji muzycznej. Autorzy wskazują na potencjał takiej wtyczki do zastąpienia, przykładowo, niezorganizowanych bibliotek sampli perkusyjnych, pozwalając na coś w rodzaju bardziej "ustrukturyzowanej" eksploracji różnych brzmień za pomocą interpretowalnych parametrów. Jest to najwcześniejsza praca podejmująca się problemu stworzenia instrumentu muzycznego z sieci neuronowej.

Dwa lata później, praca RAVE kontynuowała temat sieci neuronowej jako instrumentu również używając Variational Auto-Encoderów, aplikując jednak dużą ilość nowowpowstałych usprawnień z obszaru bezpośredniej syntezy dźwięku. Zamysłem w RAVE było stworzenie autoencodera działającego w czasie rzeczywistym na ciągłym strumieniu, co pozwalało na dwa muzyczne zastosowania:
\begin{itemize}
    \setlength\itemsep{0em}
    \item tłumaczenie na żywo dźwięku z jednej stylistyki w drugą, oraz
    \item generowanie losowego strumienia dźwięku o danej stylistyce z możliwością jego kontroli poprzez wysokopoziomowe parametry.
\end{itemize}
Zastosowane usprawnienia to:
\begin{itemize}
    \setlength\itemsep{0em}
    \item \textbf{Multi-band}: używając metody z Multi-band MelGAN, RAVE dzieli sygnał na 16 pasm, co znacząco przyśpiesza syntezę i zmniejsza końcowy rozmiar modelu.
    \item \textbf{Dwu-fazowy trening}: Autorzy wskazują na to, że dopiero taki tryb pozwolił im osiągnąć dobrą jakość dźwięku. W pierwszej fazie trenowany jest cały autoencoder, czyli następuje wyuczenie przestrzeni ukrytej, zaś w drugiej fazie dotrenowywany jest sam dekoder za pomocą \textit{adwersaryjnych funkcji strat} w celu poprawienia jakości.
    \item \textbf{Percepcyjna funkcja straty}: RAVE używa - zaproponowanej w Parallel WaveGAN i używanej później w innych pracach - funkcji straty opartej na STFT z wieloma rozmiarami okna.
    \item \textbf{Adwersaryjne funkcje straty}: W drugiej fazie treningu RAVE używa tych samych sieci dyskryminujących co MelGAN.
    \item \textbf{Komponenty DDSP}: Autorzy pożyczają komponent generujący szum z pracy Differentiable Digital Signal Processing, który jest swego rodzaju syntezatorem szumu kontrolowanym przez wyjścia z sieci, i dołączają go do bezpośrednio generowanej fali, co ułatwia rekonstrukcję wielu brzmień.
\end{itemize}
Autoencoder w RAVE, podobnie jak w wielu z wcześniej przytaczanych prac, działa na ciągu wektorów z przestrzeni ukrytej. W przypadku RAVE jest to mniej więcej 23 wektorów na sekundę, gdzie każdy z nich ma rozmiar 128. W przeciwieństwie do Neural Drum Machine, w celu bezwarunkowego wygenerowania nowego przykładu nie wystarczy wylosować pojedyńczy wektor. W związku z tym, że dla dźwięku o danej długości ciąg zakodowanych wektorów zawiera zależności czasowe, nie wystarczy również niezależne wylosowanie N takich wektorów - sieć wygenerowałaby w tym przypadku nieustrukturyzowany szum. W celu zamodelowania rozkładu \textit{prior} na ciągach wektorów z przestrzeni ukrytej, autorzy trenują na nich dodatkową konwolucyjną sieć rekurencyjną, co następnie pozwala im wygenerować spójnie brzmiące nowe przykłady.

Dla jeszcze większej poprawy efektywności i prędkości RAVE, autorzy wykorzystują obserwację, że znaczna część przestrzeni ukrytej jest praktycznie niewykorzystana w już wytrenowanej sieci. Ponieważ regularyzacja w VAE ściąga nieużywane wymiary w stronę rozkładu \textit{prior} - $\mathcal{N}(0, 1)$ - o stosunkowo dużej wariancji, sam rozkład SVD na wektorach z ukrytej reprezentacji nie pozwoliłby na zidentyfikowanie najbardziej znaczących kierunków. W związku z tym autorzy aplikują do nich najpierw dwie transformacje:
\begin{itemize}
    \setlength\itemsep{0em}
    \item \textbf{maksymalizacja rozkładu posterior}: Zamiast losować wektory ukryte z rozkładu \textit{posterior}, do analizy wybrane są przykłady maksymalizujące ten rozkład;
    \item \textbf{przesunięcie każdego wymiaru o średnią}: Wymiary bliskie do \textit{prior} po pierwszym kroku będą w przybliżeniu równe stałej wartości, czyli średniej. Po odjęciu średniej, wymiary te będą bliskie zeru.
\end{itemize}
Następnie rozkładu SVD zwraca kierunki w przestrzeni ukrytej niosące największą ilość informacji. Autorzy wprowadzają parametr \textit{fidelity}, który określa jaką część wariancji chcą zachować po kompresji. Jest to liczba z zakresu 0 do 1, którą w praktyce można interpretować jako zachowaną część oryginalnej jakości. Przykładowo, dla $f = 0.9$, reprezentacja ukryta (po rzutowaniu) jest ucięta do pierwszych N kierunków które łącznie zawierają więcej niż 0.9 całkowitej wariancji. Na dwóch zestawach danych, na których ewaluowana była sieć RAVE, wymiarowość przy $f = 0.99$ spadła ze 128 do zaledwie 24 i 16 wymiarów. Na wejściu do dekodera nieużywane wymiary są wypełniane szumem z rozkładu \textit{prior}, co odpowiada temu, jak w przybliżeniu wyglądały przed kompresją. Taki zabieg pozwala znacząco zmniejszyć złożoność rekurencyjnej sieci \textit{prior}, co ułatwia jej wykorzystanie w procesie produkcji muzyki.

Wtyczka VST udostępniona przez RAVE ma dwa tryby działania:
\begin{itemize}
    \setlength\itemsep{0em}
    \item Translacja wejścia na alternatywną stylistykę (przykładowo - śpiewu na skrzypce), gdzie docelowy styl jest determinowany przez to na czym został wytrenowany model, oraz
    \item Bezwarunkowa generacja losowego strumienia dźwięku w danym stylu na bazie dodatkowo wybranego modelu \textit{prior}.
\end{itemize}
Dodatkowo, wykorzystując wcześniej opisany mechanizm kompresji, pozwala ona na kontrolę brzmienia poprzez manipulację ośmioma najbardziej znaczącymi kierunkami w ciągu wektorów kodujących dźwięk. W trakcie generacji, dla każdego z tych kierunków można zmieniać parametry \textit{scale} i \textit{bias}, które aplikują się niezależnie do każdego wektora $z$ w sekwencji: $\hat{z} = scale * z + bias$. W zależności od modelu każdy kierunek może mieć inną interpretację, o mniej lub bardziej wyraźnej semantyce - autorzy nie przeprowadzili ewaluacji pod tym kątem. W praktyce, w przykładach pokazywanych w prezentacjach, parametry te kontrolują charakterystyki takie jak głośność, jasność czy dynamiczność dźwięku.

W podobnym czasie, gdy IRCAM badał użycie Variational Auto-Encoderów jako instrumentów, OpenAI próbował je zastosować do problemu generowania \textit{całych piosenek}. Utrzymanie spójności na przestrzeni kilku minut, jak i przejrzystej jakości wielu połączonych ścieżek instrumentalnych, wymagało innej architektury sieci. W swojej pracy Jukebox, autorzy zastosowali \textit{vector quantization} VAE: modyfikacji VAE, która kwantyzuje reprezentację ukrytą do skończonego zbioru wyuczonych, dyskretnych wektorów, których zestaw określa się nazwą \textit{codebook}. Żeby uchwycić zależności na różnych poziomach - zarówno milisekundowych, odpowiedzialnych za czystość brzmienia, wielosekundowych, odpowiedzialnych za spójność rytmiczną i harmoniczną, jak i minutowych, odpowiedzialnych za spójność kompozycji - wytrenowali trzy równoległe sieci o różnych rozdzielczościach, używając zapożyczonej z Parallel WaveGAN wieloskalowej percepcyjnej funkcji straty. Jako dekoder wykorzystali nie-autoregresywną sieć konwolucyjną typu WaveNet. Do tego, żeby model był generatywny, potrzebny był jeszcze rozkład \textit{prior} na reprezentacji ukrytej - autorzy użyli do tego trzech sieci typu \textit{sparse transformer} (TODO przypis?), modelujących najpierw najogólniejszy poziom wektorów ukrytych, a następnie kolejne dwa warunkowane na poprzednich poziomach. W ten sposób uzyskali sieć, która była w stanie, przykładowo, kontynuować dany fragment piosenki zachowując przy tym spójność w elementach takich jak stylistyka, brzmienie głosu, czy rytm. Było to przełomowe osiągnięcie w dziedzinie bezpośredniego generowania muzyki sieciami neuronowymi, które w dalszych latach ewoluowało w stronę komercyjnych rozwiązań używanych już powszechnie w branży. Biorąc pod uwagę jednak, że synteza całych piosenek wybiega poza obszar tej pracy, temat ten nie będzie tutaj dalej kontynuowany w pełnym swoim zakresie.

\section*{Kodeki neuronowe}

Kolejnym zastosowaniem Variational Auto-Encoderów w kontekście dźwięku są efektywne \textit{kodeki}, mogące znaleźć zastosowanie na przykład w oprogramowaniu do spotkań online do efektywnego przesyłu danych przez wolne połączenia. Praca SoundStream jako pierwsza użyła w pełni neuronowej architektury do uniwersalnej kompresji dźwięku, proponując przy tym rozwinięcie mechanizmu \textit{vector quantization} - \textit{residual vector quantization} - pozwalające na dynamiczną kontrolę rozmiaru ciągu wektorów ukrytych. Powstała w podobnym czasie co RAVE i zastosowała kilka podobnych rozwiązań - wieloskalową funkcję straty opartą na STFT, adwersaryjne funkcje straty i enkoder/dekoder oparte na wielu blokach rezydualnych i rozrzedzonych konwolucji. Działając jednak w innych domenach, kilka aspektów je też różniło: SoundStream nie rozdzielał treningu na dwie fazy, używał dodatkowych sieci dyskryminujących oraz działał na sygnale o częstotliwości próbkowania 24kHz bez podziału na pasma, jako że kompresja na potrzeby spotkań online raczej nie wymaga rekonstrukcji powyżej 12kHz. Dodatkowo, autorzy zaimplementowali również w samym dekoderze funkcjonalność odszumiania wejścia. Dodali do dekodera dodatkowy zero-jedynkowy sygnał warunkujący - czy odszumiać, czy nie - a następnie, podczas treningu, uczyli go rekonstrukcji już odszumionych przykładów. Poprzez swoje nowatorskie podejście, SoundStream uzyskał podobną jakość dźwięku do ówcześnie używanych kodeków przy nawet dwa razy mniejszej przepustowości.

W następnych latach kilka prac kontynuowało temat kompresji dźwięku. EnCodec usprawnił działanie SoundStream, bezpośrednio budując na jego architekturze i dodając kilka modyfikacji, takich jak zmiana adwersaryjnych funkcji straty czy dodatkowe mechanizmy kompresji wektorów ukrytych. RVQGAN połączył najważniejsze dokonania prac z linii GANowych wokoderów - HiFi-GAN i Big-VGAN, będących kontynuacjami pracy MelGAN - z SoundStream'owym enkoderem-dekoderem, żeby stworzyć sieć do uniwersalnej kompresji dźwięku o częstotliwości próbkowania 44.1kHz. Głównymi usprawnieniemi wziętymi z linii wokoderów to funkcja aktywacji \textit{snake} która wspomaga rekonstrukcję okresowych sygnałów oraz jeszcze bardziej rozwinięte sieci dyskryminujące, również dostosowane do okresowego charakteru sygnału dźwiękowego. Dodając do tego jeszcze usprawnienia w procesie uczenia się kwantyzowanej reprezentacji ukrytej, autorzy RVQGAN osiągnęli znaczną poprawę jakości nad EnCodec, zwiększając przy tym mniej więcej dwukrotnie stopień kompresji, co otworzyło drogę dla praktycznych zastosowań kompresji dźwięku w całym słyszalnym paśmie do 20kHz.

\section*{W kierunku instrumentów muzycznych i ich kontroli}

Równolegle z pracami, które rozwijały metody generowania dźwięku pod kątem jak najlepszej jakości, inny kierunek badał to, w jaki sposób sieci organizują przestrzeń ukrytą i w jaki sposób można ten proces kontrolować. O ile Neural Drum Machine a potem RAVE jedynie w skrócie analizowały semantykę najistotniejszych kierunków w przestrzeni ukrytej, praca Generative Timbre Spaces z 2018 roku sięgnęła po dokonania z dziedziny akustyki, aby tę przestrzeń ukrytą ustrukturyzować w sposób interpretowalny dla człowieka. Używają koncepcji \textit{timbre spaces} - iluś-wymiarowej przestrzeni, w której zanurzone są brzmienia przykładowo instrumentów, odzwierciedlającej podobieństwa i różnice tego jak je postrzegamy. Taki \textit{timbre space} jest konstruowany na podstawie subiektywnych testów, w której zestaw uczestników ocenia każdą przedstawioną mu parę brzmień na ciągłej skali od 0 do 1, gdzie 0 oznacza, że są identyczne, a 1, że są w największym stopniu różne. Z tak uzyskanej macierzy odległości, po wybraniu danego N, konstruowana jest dla każdego przykładu jego reprezentacja w N-wymiarowej przestrzeni. Autorzy Generative Timbre Spaces używają \textit{timbre spaces} z kilku prac z lat 90-tych i tworzą ujednoliconą przestrzeń zawierającą 12 instrumentów, a następnie trenują VAE na zestawie przykładów z tych instrumentów z dodatkowym elementem funkcji straty. Jest on zdefiniowany poprzez \textit{Kullback-Leibler Divergence} (KLD) między
\begin{itemize}
    \setlength\itemsep{0em}
    \item rozkładem odległości danego przykładu do innych przykładów w przestrzeni ukrytej, a
    \item rozkładem odległości instrumentu tego przykładu do instrumentów innych przykładów w przestrzeni \textit{timbre space}.
\end{itemize}
Wytrenowana sieć jest regularyzowana więc w taki sposób, żeby wyuczona przestrzeń ukryta miała podobną strukturę do zadanego \textit{timbre space}. Pozwala to na pewne uogólnienie zadanej przestrzeni i zbadanie przykładowo tego, w jaki sposób instrumenty spoza zestawu danych się w niej umiejscawiają.

Kwestię kontroli przy generowaniu brzmień perkusyjnych poruszyła praca Neural Percussive Synthesis z 2019 roku. W przeciwieństwie do NDM, zamiast pozwolić samej sieci ukształtować przestrzeń ukrytą, jako pierwsza explicite warunkowała proces generacji na wcześniej obliczonych własnościach dźwięku. Są to: \textit{boominess}, \textit{brightness}, \textit{depth}, \textit{hardness}, \textit{roughness}, \textit{sharpness} i \textit{warmth}, wyciągnięte algorytmicznie za pomocą ogólnodostępnego oprogramowania. Dodatkowo, na wejściu była pełna obwiednia określająca głośność dźwięku w czasie. Autorzy odeszli od architektury generatywnej stosując sieć typu U-Net i trenując ją jako deterministyczną transformację z obwiedni i obliczonych cech do fali dźwiękowej. Otrzymali w ten sposób swego rodzaju syntezator pozwalający na ciągłą i niezależną kontrolę nad zestawem siedmiu parametrów, wykazując jednocześnie, że sieć rzeczywiście poprawnie odzwierciedla wejście warunkujące w zsyntezowanej fali.

Praca DrumGAN z 2020 roku kontynuowała temat kontroli poprzez ustalone cechy, wracając jednak do generatywnych architektur w celu uchwycenia wariacji, jaką mieć mogą różne brzmienia mimo podobnych własności. Używając tych samych 7 cech co Neural Percussive Synthesis, autorzy wytrenowali sieć typu \textit{Progressive Growing GAN} na zestawie brzmień perkusyjnych należących do jednej z trzech klas - stopa, werbel, lub hi-hat. Sygnał warunkujący dołączyli poprzez prostą konkatenację do 128-wymiarowego wektora szumu na wejściu do generatora. Na wyjściu, podobnie jak w GANSynth, sieć generowała ciąg spektrogramów z dodatkową informacją o fazie, jednak w innej - bardziej bezpośredniej - reprezentacji, z której następnie dźwięk był syntezowany poprzez odwróconą transformatę Fourier'a. Dzięki temu autorzy uzyskali model generatywny pozwalający na syntezę losowego z trzech rodzajów brzmień perkusyjnych, z możliwością jego kontroli poprzez 7 ustalonych cech. Do ewaluacji swojego rozwiązania użyli:
\begin{itemize}
    \setlength\itemsep{0em}
    \item \texit{Inception Score}, podobnie jak w poprzednich pracach, oceniające jak wyraźnie dany przykład należy do jednej z trzech klas;
    \item \textit{Frechet Audio Distance}, czyli {Frechet Inception Distance} dostosowany do dźwięku poprzez użycie innej sieci do embeddingów;
    \item \textit{Feature coherence test}, czyli procedura z Neural Percussive Synthesis mającą określić, na ile kontrola ustalonych cech na wejściu jest poprawnie odzwierciedlona w wygenerowanym brzmieniu;
    \item {Kernel Inception Distance}, czyli metodzie pożyczonej z prac w dziedzinie obrazu podobnie jak FID opartej na embeddingach wyciągniętych z klasyfikatora typu Inception.
\end{itemize}
Z uwagi na nie-generatywny charakter NPS, autorzy mogli ilościowo porównać jedynie \textit{feature coherence}. DrumGAN osiągnął w tym gorszy wynik, jednak autorzy wskazują przy tym na dwie rzeczy: (a) generatywny charakter rozwiązania naturalnie utrudnia sieci łatwe dostosowanie się do sygnału warunkującego, oraz (b) subiektywna jakość wygenerowanych przez DrumGAN przykładów jest według nich lepsza, co również może być kwestią tej architekturalnej zmiany. Za pomocą innych metryk wykazali, że wersja sieci bez sygnału warunkującego osiąga gorszą jakość na wyjściu, oraz to, że kombinacje cech odbiegająca od tych widzianych w zestawie treningowych wciąż syntezują przykłady o wysokiej jakości.

Następna praca od tych samych autorów, DrumGAN VST, zwiększyła częstotliwość próbkowania z 16 do 44.1 kHz i dodała osobny enkoder rzutujący istniejące przykłady na przestrzeń ukrytą, czyli wektor szumu wraz z sygnałem warunkującym. Zamiast 7 algorytmicznie obliczonych cech, po wysłuchaniu opinii od ludzi z branży, autorzy zdecydowali się jako sygnał warunkujący dać klasy instrumentów - stopę, werbel i hi-hat. Mimo tego, że są to kategoryczne zmienne, wytrenowali nową sieć w ten sposób, żeby można było je kontrolować na ciągłej skali, pozwalając na syntezę brzmień stanowiących ich mieszankę. Jako pierwsza tego typu sieć, DrumGAN VST stała się częścią komerycyjnego wirtualnego syntezatora perkusyjnego - \textit{Backbone 1.5} od firmy Steinberg. Wtyczka ta pozwala na projektowanie swoich brzmień składając je z ośmiu warstw, gdzie każda warstwa może być załadowana z pliku lub wygenerowana losowo za pomocą sieci. W obu przypadkach umożliwia następnie modyfikację warstwy poprzez zmianę parametrów \textit{stopa}, \textit{werbel} i \textit{hi-hat}. Autorzy porównali jakość generowanych brzmień z dwiema powstałymi w międzyczasie pracami - CRASH, próbującej zaaplikować nowatorski mechanizm dyfuzji do problemu generowania brzmień perkusyjnych, oraz Style-DrumSynth, do tego samego problemu aplikującą nową adwersaryjną architekturę StyleGAN. Metryki FAD i KID wykazały, że DrumGAN VST osiąga najlepszą jakość.

Równolegle, w dziedzinie przetwarzania obrazu, seria prac eksplorowała aspekt kontroli poprzez nienadzorowaną analizę najistotniejszych kierunków w przestrzeni ukrytej w GANach. W przeciwieństwie do Variational Auto-Encoderów, GANy w swojej standardowej wersji nie umożliwiają rzutowania dowolnego przykładu na swoją przestrzeń ukrytą. Sam rozkład wektora szumu - $\mathcal{N}(0, 1)$ - nie zawiera w sobie informacji, z których dałoby się wyczytać istotne kierunki. Powstało kilka sposobów na to, żeby wyczytać je z już wytrenowanej sieci:
\begin{itemize}
    \setlength\itemsep{0em}
    \item \textbf{Dodatkowy wytrenowany enkoder} - PCA (lub inne metody analizy najważniejszych kierunków) na zestawie przykładów rzutowanych na przestrzeń ukrytą, podobnie jak w przypadku VAE; tę metodę stosuje Style-DrumSynth.
    \item \textbf{Pośrednie warstwy} - Ta metoda opiera się na obserwacji, że wczesne warstwy sieci generującej już kodują ważne cechy powstałego dźwięku. Polega na obliczeniu PCA (lub innej metody) na pośredniej reprezentacji zestawu \textit{syntetycznych} przykładów wylosowanych z przestrzeni ukrytej, po przepuszczeniu ich przez wybrane warstwy sieci. Kierunki są następnie rzutowane z powrotem na samą przestrzeń ukrytą poprzez regresję liniową.
    \item \textbf{Semantic Factorization} (\textbf{SeFa}) - Ta metoda, podobnie jak poprzednia, próbuje wyciągnąć ważne kierunki z wczesnych warstw sieci generującej. Zamiast jednak działać na prawdziwych bądź syntetycznych przykładach, robi to za pomocą obliczeń bezpośrednio na wagach pierwszej warstwy, wykorzystując to, że w standardowych GANach jest to transformacja liniowa.
\end{itemize}
Praca (przypis - study of control methods for percussive sound synthesis) z 2022 roku spróbowała zaaplikować trzecią metodę do syntezy brzmień perkusyjnych i porównać ją do takiej używającej ustalonych cech. Żeby ustalić bazowy poziom, czyli taki przy kontroli bez wyciągania żadnych kierunków, dodali do tego jeszcze bezpośrednią modyfikację każdego z 512 wymiarów przestrzeni ukrytej. Do ewaluacji stworzyli prosty interfejs pozwalający na syntezę wytrenowaną siecią i kontrolę jedną z trzech metod, a następnie przedstawili go 14 ludziom z różnym doświadczeniem w produkcji muzyki. Każdy uczestnik następnie wypełnił ankietę \textit{Creative Support Index}, która służy do badania "systemów wspierających kreatywność" i na podstawie odpowiedzi na różne pytania na skali 0-10 (zupełnie się nie zgadzam/zdecydowanie się zgadzam) ocenia narzędzie w sześciu różnych wymiarach tego wsparcia. Sumarycznie, \textit{SeFa} osiągnęła wyższy wynik niż pozostałe dwie metody, jednak autorzy wskazują na to, że po wzięciu pod uwagę przedziałów ufności żadna z metod nie jest lepsza w statystycznie istotny sposób. Opierając się jednak na rozmowach z użytkownikami twierdzą, że kierunki znalezione przez \textit{SeFa} były często raportowane jako jasno interpretowalna i percepcyjnie niezależne od siebie.
