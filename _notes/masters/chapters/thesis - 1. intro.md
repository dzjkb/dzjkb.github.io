# conceptual intro
**evolution of technology vs the evolution of instruments vs the creative process**

Synteza dźwięku jest kluczowym elementem produkcji muzyki.
Rozwój (eletronika itd) w latach (??) (coraz bardziej dostępnej zwykłym ludziom) przyczynił się do (fundamental) zmiany w brzmieniu muzyki popularnej.
Inne niż dotychczas sposoby syntezy dźwięku wiązały się z innym procesem kreatywnym, przez co same narzędzia i to w jaki sposób są zaprojektowane zaczęło wpływać na brzmienie ówczesnej muzyki. Dotyczy to zarówno tej popularnej jak i bardziej eksperymentalnej, w której większy nacisk bywa położony na relacji z instrumentem i eksploracji tego jakie daje on możliwości ekspresji.
Niektóre syntezatory same w sobie zdefiniowały nowe gatunki, stając się istotnymi elementami kultury aniżeli tylko narzędziami (jeff mills i jego perfo na 303 czy tam ileś).
Wraz z rozwojem technologii komputerowych zaczęły powstawać coraz to nowsze narzędzia do syntezy lub manipulacji dźwięku. Rozpowszechnienie osobistych komputerów ze stosunkowo dużą mocą obliczeniową wiązało się z rozwojem instrumentów czysto cyfrowych i dalszą ewolucją interfejsów syntezy dźwięku, a nowe interfejsy wiązały się z jeszcze bardziej nowatorskimi i nieograniczonymi(?) sposobami ekspresji.
cośtam cośtam relacja instrument interfejs ekspresja proces kreatywny

**kontrolowanie syntezy**

tak w skrócie
- oscylatory, filtry
- na czym polega subtraktywna/granularna/addytywna?
głos - wokodery, muzyka - synthy

**synteza sieciami neuronowymi**

sieć też jest takim syntezatorem! ale bez pokręteł
ale możemy dodać pokrętła?
zmiana paradygmatu - kontrola wysokopoziomowych cech
(i to wynalezionych z danych a nie zaprogramowanych wcześniej)
# generative models in music synthesis brief
Piewrsze prace nad syntezą muzyki w sposób end-to-end za pomocą sieci neuronowych zaczęły się kilka lat po tym jak sieci neuronowe zaczęły wchodzić do mainstream'u w dziedzinie przetwarzania obrazu.
[[WaveNet]] pierwsza fundamentalna praca modelująca _raw audio_ inspirowana przez podobnie wysokowymiarowe rozwiązania dla obrazu
skupienie na głosie jako że to ważne, ale równolegle eksplorowano muzyczne zastosowania
- 2017: tacotron, nsynth
- 2018: tacotron2, SING
- 2019: MelGAN, GANSynth, Neural Drum Machine
- 2020: już stricte muzycznie - Multi-band MelGAN (towards real-time), DrumGAN, Jukebox; głosowo - ?
- 2021: RAVE, Crash, SoundStream (codecs)
- 2022: DrumGAN VST (practical usage), EnCodec (codecs)
- 2023: CLAP embeddings (multimodality), RVQGAN (marrying VAE and GAN methods), MusicLM (multimodality)
- 2024:
- 2025: DRAGON (diffusion), CALM (multimodality), Platune (creative), that guys phd (he's the second author in Platune)
obecnie:(codecs)
- 2022: DrumGAN VST (practical usage), EnCodec (codecs)
- 2023: CLAP embeddings (multimodality), RVQGAN (marrying VAE and GAN methods), MusicLM (multimodality)
- 2024:
- 2025: DRAGON (diffusion), CALM (multimodality), Platune (creative), that guys phd (he's the second author in Platune)
obecnie:
- skręcanie w stronę closed-source w związku z coraz realniejszym biznesowym wykorzystaniem
- fuzja z modelami językowymi - multimodalność, koncepcyjne zrozumienie dźwięku (MusicLM, Suno, ?)
- eksploracja modeli dyfuzyjnych
- kreatywne narzędzia z wykorzystaniem AI - [[that guys phd]] etc
# most important results brief
W tej pracy proponuję nową modyfikację architektury sieci pozwalającą na wykorzystanie jej jako szczególnego rodzaju muzycznego syntezatora. Istotnym elementem jest obecny w niej aspekt kontroli wysokopoziomowych parametrów brzmienia, który umożliwia włączenie narzędzia do kreatywnego procesu.
Przede wszystkim pokazuję, że:
- architektura taka jest w stanie osiągnąć niegorszą jakość dźwięku od podobnych rozwiązań
- cechy wyeksponowane przez taki syntezator mają wyraźną interpretację i pozwalają na różnorodną kontrolę powstałego dźwięku.
W porównaniu z rozwiązaniem robiącym to samo ale opartym na GANach: bardziej interpretowalne cechy (więcej)

**side note - narrative**
wait am I the first to do a music VAE with a single latent?
this enables:
- "global" features
- analysis on the prior structure, as opposed to rnn priors

---
# tekst

\section*{Synteza dźwięku}

Synteza dźwięku jest kluczowym elementem produkcji muzyki. Poprzez opracowywanie coraz nowszych metod syntezy, rozwój elektronicznych technologii w drugiej połowie XX wieku przyczynił się do fundamentalnych zmian w tym, jak brzmiała ówczesna muzyka. Na swój sposób czerpała z tego muzyka eksperymentalna - aktywnie szukająca nowych form ekspresji i badająca możliwości artystycznego wykorzystania najnowszych technologii - jak i muzyka popularna, opierająca czasem brzmienie całego nowego gatunku na jednym konkretnym nowym instrumencie.

TODO: enaktywizm

Takim przełomowym instrumentem był na przykład syntezator perkusyjny Roland TR-808. Tuż po wydaniu wcale nie odniósł sukcesu - konkurencyjne maszyny wierniej oddawały prawdziwe brzmienia perkusyjne. Przez swoją niską cenę i charakterstyczne, inne brzmienie, szybko jednak zaczął rozprzestrzeniać się wśród niszowych artystów i stał się bazą do powstającego wtedy hip-hopu i tanecznej muzyki elektronicznej. Podobny los spotkał maszyny TR-909 oraz TB-303, której wydanie The Guardian okreśił jednym z [50 najważniejszych wydarzeń muzyki tanecznej](https://www.theguardian.com/music/2011/jun/15/tadao-kikumoto-roland).

Nowe sposoby syntezy dźwięku mają fundamentalny wpływ na kształt nowej muzyki nie tylko poprzez swoje unikalne brzmienie. Muzykę kształtuje również sposób - interfejs - w jaki dany instrument pozwala ze sobą wchodzić w interakcję. Tworzenie muzyki jest procesem eksploracji w medium - w używanych narzędziach, instrumentach - i sposoby czy możliwości interakcji z tym medium naturalnie kształtują to, co będziemy w stanie podczas takiej eksploracji wytworzyć. (fragment o musique concrete?) Wywodząca się z Musique Concrete kompozytorka Eliane Radigue poświęciła w ten sposób 25 lat swojej kariery na eksplorację syntezatora ARP 2500, pisząc w latach 1974-1999 prawie wyłącznie kompozycje na nim się opierające. (dron bo ARP 2500? citation needed)

W późniejszych latach, równolegle z coraz to nowszym elektronicznym sprzętem muzycznym, rozwijać zaczęły się software'owe narzędzia do syntezy. Interfejs osobistego komputera dostarczył jeszcze to nowszych sposobów na pracę z dźwiękiem. Syntezatory analogowe zaczęto implementować w wersjach cyfrowych, co pozwoliło na jeszcze większe możliwości kontroli parametrów tej syntezy oraz interakcji między różnymi systemami - syntezatorami, kontrolerami.

\section*{algorytmy syntezy i kontrola}

Pierwsze instrumenty elektronicze opierały się na sposobie syntezy zwaną \textit{syntezą subtraktywną}. W najogólniejszym sensie polega ona na:
\begin{enumerate}
    \setlength\itemsep{0em}
    \item wytworzeniu dźwięku za pomocą \textit{oscylatorów} - układów wchodzących w oscylacje harmoniczne, tworzące fale o różnych kształtach (np. sinusoidalna, kwadratowa, piłokształtna) i o określonych częstotliwościach, a następnie
    \item przetworzeniu tej fali przez \textit{filtry}, \textit{obwiednie} lub inne układy kształtujące dźwięk.
\end{enumerate}
Oscylatorów i innych modułów jest zwykle po kilka, połączonych w różne sposoby zależnie od brzmień jakie chce się uzyskać. Najczęściej instrumenty takie mają również klawiaturę na kształt fortepianowej, za pomocą której osoba grająca kontroluje wysokość i długość trwania.

Oprócz długości i wysokości, istotna jest również kontrola samego brzmienia. Tradycyjnie syntezatory umożliwiają to za pomocą różnych pokręteł i przycisków, które bezpośrednio kontrolują ustalone parametry układu od syntezy. Biorąc podstawowy układ z wcześniejszego akapitu, składający się z kilku oscylatorów, filtra i obwiedni - jedno pokrętło może odpowiadać za odcięcie filtra, drugie za rezonans, a zestaw guzików w liczbie odpowiadającej oscylatorom może zmieniać kształt ich fali między trójkątną a piłokształtną. Bardzo często na takich syntezatorach występuje też zestaw pokręteł kontrolujących ostatnią obwiednię kształtującą głośność dźwięku w czasie, z klasycznymi oznaczeniami "attack", "sustain", "decay" i "release" (stąd nazwa "obwiednia ASDR").

Używającj takiego interfejsu, osoba grająca na syntezatorze zmienia parametry syntezy i kształtuje brzmienie według swojej wizji. Pole do manewru przy tworzeniu takiego interfejsu jest ogromne - oprócz pokręteł czy guzików można wykorzystywać na przykład elementy czułe na nacisk, slidery, akcelerometry, czy nawet wejścia zewnętrznych sygnałów kontrolnych. Różnorodność jest ogromna, a ostateczny wygląd takiego interfejsu i to jak się z niego korzysta bezpośrednio wpływa na to jak będą na nim wyglądały performance'y i jakie brzmienia będzie można wydobyć.

Software'owe narzędzia do syntezy również cieszą się ogromną różnorodnością powstających interfejsów. Cyfrowy algorytm syntezy da się kontrolować nie tylko za pomocą programowych odpowiednikow pokręteł czy guzików, ale też za pomocą dowolnego innego sygnału który przetłumaczymy na rozpoznawalny dla programu format. Oznacza to, że możliwe staje się na przykład wykorzystanie czujników ruchu, światła, dźwięku, czy algorytmów wyczytujących informacje z kamery. Taki sygnał następnie można w dowolny sposób podpiąć do algorytmu syntezy, tworząc brzmienie które bezpośrednio kształtują warunki w pomieszczeniu lub czyiś ruch, gesty, czy nawet mimika twarzy.

\section*{nowe sposoby syntezy}

W ogólności można powiedzieć, że do stworzenia instrumentu elektronicznego potrzebne są przede wszystkim dwa elementy: algorytm syntezy i sposób na jego kontrolę. Pierwsze syntezatory software'owe emulowały analogowe instrumenty, implementując w kodzie przybliżenia oscylatorów czy filtrów. Dziedzina cyfrowa umożliwia jednak wiele innych sposobów na wygenerowanie dźwięku. 

Jednym z takich nowych rodzajów syntezy który zyskał ogromną popularność ze względu na swoją wszechstronność to \textit{synteza granularna}. Zamiast bazować na oscylatorach, źródlem dźwięku w syntezie granularnej jest dowolny, nagrany już klip dźwięku. Następnie, klip ten jest odtwarzany w bardzo małych fragmentach (granulkach) z różnych pozycji, potencjalnie odtwarzając wiele takich granulków naraz. Granulki mogą też odtwarzać fragmenty w innym tempie (czy nawet wstecz) lub tworzyć między sobą rytmiczne kombinacje. Efektywnie jest to sposób na rekombinację istniejącego nagrania - czegoś potencjalnie bardzo prostego - w zupełnie nowe, bogate i wielowarstwowe brzmienia.

Innym sposobem są sieci neuronowe. W 2016 roku, w przełomowej pracy WaveNet \cite{wavenet}, autorzy podjęli się zadania syntezy mowy bezpośrednio za pomocą jednowymiarowych sieci konwolucyjnych. Pokazali, że takie podejście stanowczo przebija dotychczasowe systemy TTS (text to speech), co stworzyło fundament pod kolejne 10 lat badań nad generowaniem dźwięku z użyciem sieci neuronowych. Zgodnie z naturą sieci neuronowych, algorytm syntezy staje się w tej sytuacji black boxem i nie jesteśmy w stanie w oczywisty sposób zdefiniować parametrów do jej kontroli. Każdy neuron, teoretycznie, jest parametrem algorytmu, jednak kontrola wartości pojedyńczych neuronów nie wpłynęłaby znacząco na wyjściowe brzmienie. Różne architektury jednak pozwalają na różne sposoby bardziej wysokopoziomowej kontroli, poczynając już od WaveNetu który jako system TTS ma sygnał warunkujący syntezę w postaci tekstu, który chcemy usłyszeć oraz tożsamości "mówcy". W pracach wykorzystujących Variational Autoencodery czy GANy kontrola ta odbywa się przez przestrzeń ukrytą, czyli niskowymiarową przestrzeń z której wektory sieć transformuje - lub dekoduje - żeby uzyskać dźwięk.

Kontrola sieci neuronowej poprzez tego typu metody ma fundamentalnie inny charakter niż sposoby kontroli w klasycznych, software'owych czy analogowych syntezatorach. Tradycyjnie, kontrola ta eksponuje bardzo konkretne techniczne parametry wynikające z tego jak działa dany moduł, na przykład odcięcie filtra czy kształt fali. W przypadku sieci parametry te stają się wysokopoziomowymi, rozmytymi konceptami, jak chociażby tożsamość mówcy w sieci WaveNet czy "jasność" brzmienia w DrumGAN. Pozwala to na zupełnie inny sposób interakcji z takim narzędziem, co kształtuje proces twórczy i sztukę jaka powstaje z jego użyciem.

\section*{najważniejsze wyniki}

W tej pracy proponuję nową architekturę sieci do generowania krótkich, muzycznych dźwięków pod wykorzystanie jako instrument. Bazuję na architekturze Variational Autoencoder podobnej do tych wykorzystanych w RAVE czy SoundStream i ewaluuję ją na trzech własnoręcznie stworzonych zestawach danych. Pokazuję, że:
\begin{itemize}
    \setlength\itemsep{0em}
    \item potrafi generować dźwięk o wysokiej jakości porównywalny do innych podejść, oraz
    \item proces syntezy może być kontrolowany poprzez wysokopoziomowe interpretowalne parametry zakodowane w przestrzeni ukrytej.
\end{itemize}
Dodatkowo porównuję strukturę przestrzeni ukrytej pod kątem kontroli z pracą DrumGAN, pokazując, że zaproponowana architektura daje w wyniku bardziej interpretowalne cechy. Na koniec krótko omawiam w jaki sposób tak wytrenowana sieć może zostać opakowana we wtyczkę VST, gotową do użycia w dowolnym środowisku do produkcji muzycznej.