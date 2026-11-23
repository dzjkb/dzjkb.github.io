## cyfrowe reprezentacje dźwięku
nieużywane - symboliczne (midi), spektrogramy (frequency domain)
samplowana+kwantyzowana fala dźwiękowa
## Fourier Transforms
damn I could write and write about this shit (not from memory ofc)
most important things:
- complex output (phase) vs power/magnitude
- continuous vs. discrete vs. short time (windowed)
- linear vs. mel scale

ok so uhhhhhhhhhh
- Fourier Series is an infinite series at some frequencies (not sure how the resolution is chosen?)
- Fourier Transform is the generalization of that into continuous freq
- Discrete-time FT is based on sampling, since (based on the sampling theorem) you can reconstruct some continuous functions perfectly
- Discrete FT is FT in discrete time/length
## PQMF
introduced in [[Parallel WaveGan]] and later used in [[Multi-band MelGAN]]
ciężka matma ale niedużo wsm po prostu opisać
i że poszczególne pasma mają nagle o wiele mniejszą złożoność => real-time jest możliwe
[DurIAN](https://arxiv.org/pdf/1909.01700) has a nice appendix about this and a ref

ok fuck I don't know shit about this
ok thx Kagi u the man
## essentia
briefly about how this extracts things, classical algos or sum idk
https://essentia.upf.edu/

this is just a collection of DSP algorithms, not even described that much
what can I write about this
## modele generatywne
https://www.deeplearningbook.org/contents/mlp.html
- page 197 - works to cite for deeper -> better
https://www.deeplearningbook.org/contents/generative_models.html
- 690 onwards

podstawy VAE - [Kingma et al](https://arxiv.org/pdf/1312.6114)
- idea, problem
- troche matmy
- variational inference
- własności
podstawy GANów - ?
- idea, problem
- uczenie z siecią dyskryminującą
- znane problemy i ich rozwiązania w dalszych iteracjach
optymalizacja Adam?

variational inference methods - one that approximate the posterior because it's intractable ok ok ok
standard EM, in the expectation step, needs $p(z|x, \theta^t)$
- how is that defined? isn't is already parametrized or some shi?
- right, in the example of gaussian mixtures it's from the bayes theorem
- so closed form somehow
## Pytorch
maybe? idk
## normalizing flows
normalizing flows - sposób na uczenie się rozkładów
troche matmy, troche wyników z pierwszych paperów
są używane w kilku rolach w modelach generatywnych - albo jako same generatory dźwięku albo jako bardziej ekspresywne rozkłady prior/posterior
opis jako część generatorów
opis prior/posterior zastosowań
pojawiają się czasem w nowszych pracach
różne typy, w szczególności RealNVP

[[normalizing flows]]

https://arxiv.org/pdf/1912.02762v2
# SeFa

---
# text

\section*{Cyfrowa reprezentacja dźwięku}

Fala dźwiękowa, będąc ciągłym, analogowym fenomenem, nie daje się w idealny sposób reprezentować cyfrowo. Matematycznie rzecz biorąc, jest to zmiana ciśnienia w czasie, czyli funkcja z liczby rzeczywistej w liczbę rzeczywistą. Cyfrowy zapis wymaga kwantyzacji obu tych wielkości, co w wymiarze czasu dzieje się poprzez \textit{próbkowanie} sygnału w stałych odstępach czasu, a w drugim poprzez kwantyzację próbkowanej wartości z ustaloną rozdzielczością. Przykładem takiego zapisu może być format \textbf{WAV}, 16-bitowy w 44.1 kHz. Jest to nieskompresowany zapis fali w postaci 44100 próbek na sekundę, gdzie każda próbka jest 16-bitową wartością, a kwantyzacja rozkłada wszystkie możliwe wartości w równych odstępach. (TODO obrazek ten z wavenetu może) Grafika nr ??? schematycznie obrazuje taki zapis.

Próbkowanie sygnału, w ogólności, jest operacją stratną. Zgodnie jednak z twierdzeniem Nyquista-Shannona, przy danej częstotliwości próbkowania $\mathcal{f}$, sygnały o maksymalnej częstotliwości składowej mniejszej niż $f / 2$ da się z takiej reprezentacji idealnie z powrotem zrekonstruować. Częstotliwości ponad tą granicą po próbkowaniu dają w wyniku identyczną reprezentację co pewne częstotliwości poniżej granicy, co uniemożliwia jednoznaczne odwrócenie operacji (fenomen ten nazywa się \textit{aliasingiem} \footnote{https://pl.wikipedia.org/wiki/Aliasing_(przetwarzanie_sygna%C5%82%C3%B3w)}). Patrząc na to w drugą stronę, dla sygnału z maksymalną częstotliwością składową $b$, jego bezstratny zapis cyfrowy wymaga częstotliwości próbkowania równej lub większej niż $2b$. W przypadku dźwięku, ludzki zakres słyszalności wynosi 20Hz - 20kHz, przez co jego bezstratne próbkowanie wymaga częstotliwości większej niż 40kHz. Z przyczyn historycznych powszechnie używaną częstotliwością w cyfrowym zapisie dźwięku, jak na przykład na płytach CD, jest 44.1kHz. Obecnie używa się również często 48kHz, a w kontekście studyjnym zdarza się nawet 96kHz lub 192kHz, co redukuje ryzyko artefaktów związanych z 

Współczesne formaty zapisu dźwięku, takie jak \textit{flac} czy \textit{mp3}, wybiegają daleko poza zwykłe zapisywanie próbek. Korzystając z różnych metod kompresji zapisują praktycznie nierozróżnialny od oryginału dźwięk w kilkukrotnie lżejszych plikach.

Teoretycznie, kwantyzacja wartości próbek nie musi rozkładać je w równych odstępach. W oryginalnym WaveNet, autorzy zamiast na wyjściu sieci modelować 16-bitową wartość - 65536 możliwych wartości - kwantyzują wyjście do 256 możliwych wartości, które następnie przekształcają w próbki używając transformacji $\mu$-law:
$$f\left(x_t\right) = \operatorname{sign}(x_t) \frac{\ln\left(1 + \mu \left|x_t\right|\right)}{\ln\left(1 + \mu\right)}$$gdzie $\mu = 255$, a $-1 < x_t < 1$. W praktyce zwiększa to rozdzielczość w wartościach bliskich 0 kosztem wartości na granicach zakresu, co według autorów znacznie poprawie jakość rekonstrukcji w porównaniu ze zwykłą liniową kwantyzacją.

Oprócz bezpośredniego zapisu fali, istnieje też wiele sposobów przedstawiania dźwięku operujących na innych poziomach abstrakcji. Konkretny format zależy od funkcji, jaką ma spełniać i dziedziny której dotyczy. Przykładowo, dla ludzkiej mowy, istnieje \textit{międzynarodowy alfabet fonetyczny} \footnote{\url{https://en.wikipedia.org/wiki/International_Phonetic_Alphabet}} służący do precyzyjnego zapisu wymowy. W muzyce utwory zapisuje się nutami, które abstrahują od wielu aspektów końcowego wykonania danego dzieła. W cyfrowej produkcji muzyki stosuje się format MIDI - coś podobnego do zapisu nutowego, lecz lepiej przystosowanego do odczytu komputerowego. Wyższy poziom abstrakcji pozwala pominąć niepotrzebne szczegóły i zapisać pewne rzeczy w mniejszej ilości informacji, co przekłada się następnie na efektywniejsze modelowanie danych - przykładem tutaj mogą być prace zajmujące się generowaniem melodii, operujące na formacie MIDI, dla których aspekty takie jak użyty instrument czy akustyka pomeszczenia nie są istotne. Praca Wave2Midi2Wave od Magenty \cite{hawthorne2019maestro} używa MIDI jako pośredniej reprezentacji do generowania wykonań na fortepianie - ich sieć generuje najpierw sekwencję MIDI, która następnię jest sygnałem warunkującym dla modelu opartego na WaveNet generującego wykonanie na fortepianie.

\section*{Transformata Fourier'a}

Transformata Fourier'a jest fundamentalną dla dziedziny przetwarzania sygnału transformacją przekształcającą funkcję z dziedziny czasu w dziedzinę częstotliwości. Opiera się na obserwacji, że dowolny (nawet nieokresowy) sygnał da się wyrazić przez sumę bazowych częstotliwości - fali sinusoidalnych. Wynikiem transformaty dla danej częstotliwości jest liczba zespolona, kodująca w sobie informację o natężeniu i fazie tej składowej, co pozwala na odwrócenie operacji i zrekonstruowanie oryginalnego sygnału (TODO przypis czy co).

Dla zdyskretyzowanych sygnałów, próbkowanych w równych odstępach czasu, wariacją jest \textit{dyskretnoczasowa transformata Fourier'a} (DTFT). Zgodnie z wcześniej przytoczonym twierdzeniem Nyquista-Shannona, według którego sygnał o jakiejś masksymalnej częstotliwości składowej może zostać idealnie zrekonstruowany przy dwa razy częstszym próbkowaniu, DTFT jest w stanie idealnie przybliżyć transformatę dla sygnałów spełniających ten warunek. DTFT działa na wejściu dowolnej długości, rozkładając je w całości na składowe. W praktyce, działając na strumieniu dźwięku, interesującą wielkością jest ewolucja częstotliwości składowych w czasie. Do tego służy \textit{krótkoczasowa transformata Fourier'a} (STFT), która aplikuje DTFT w krótkich oknach czasowych przesuwanych po sygnale. O ile pozwala to na analizę w czasie, zmniejszona liczba próbek na wejściu do transformaty pogarsza precyzję odczytu częstotliwości odwrotnie proporcjonalnie do długości okna. Daje to parametr, którym można kontrolować kompromis między precyzją odczytu STFT w czasie versus w częstotliwościach.

\section*{PQMF}

Zestaw filtrów PQMF - \textit{Pseudo-Quadrature Mirror Filters} - to algorytm rozdzielania dźwięku na wiele pasm oraz ich syntezy z powrotem w jeden sygnał. Będąc algorytmem cyfrowego przetwarzania sygnału, wybiega poza dziedzinę tej pracy, przez co jedynie powierzchowny obraz zostanie nakreślony w tej pracy.

Filtry PQMF to bank $N$ filtrów dolno-przepustowych z modulacją kosinusową, każdy wyciągający z sygnału jedno pasmo o $N$ razy mniejszej częstotliwości próbkowania. Parametry indywidualnych filtrów dobrane są w ten sposób, aby:
\begin{itemize}
    \setlength\itemsep{0em}
    \item każda częstotliwość była opóźniana o tę samą wartość, co znaczy że są \textit{liniowofazowe},
    \item aliasing graniczących ze sobą filtrów znosił się nawzajem, oraz
    \item charakterystyka filtrów graniczących ze sobą, w miejscach gdzie się nakładają, sumował się do wejściowej amplitudy,
\end{itemize}
przez co jedyne artefakty które zostają po powrotnej syntezie sygnału to aliasing niegraniczących ze sobą pasm, których amplituda zależy od stopnia tłumienia składowych filtrów. W przypadku Multiband-MelGAN jest to -70db, a w RAVE oraz w tej pracy użyte jest -100db, co znaczy, że w praktyce artefakty te nie są słyszalne. Wykres (TODO wykresik) przedstawia charakterystykę częstotliwościową zestawu 4 filtrów PQMF z Multiband-MelGAN.
![[Pasted image 20261119161023.png]]

\section*{Deskryptory dźwięku - essentia}

Zestawy danych użyte w tej pracy mają bardzo ograniczony zestaw metadanych przypisanych do każdego z przykładów. Do oznaczenia danych nie trzeba używać jednak klasyfikatorów czy ludzkich, subiektywnych ocen - w dziedzinie przetwarzania sygnału istnieje wiele opracowanych algorytmów analizujących dźwięk pod kątem na przykład intensywności, złożoności czy harmoniczności. Paczka \textit{essentia} \footnote{https://essentia.upf.edu} implementuje wiele z nich, pozwalając na obliczenie dla jakiegoś fragmetu dźwięku cech takich jak:
\begin{itemize}
    \setlength\itemsep{0em}
    \item złożoność dynamiczna,
    \item wyrazistość wysokości dźwięku,
    \item stosunku parzystych/nieparzystych harmonicznych,
    \item kształt obwiedni,
    \item centroida, czy
    \item stosunek niskich/wysokich częstotliwości.
\end{itemize}

(TODO: jeszcze coś?)

\section*{Deep learning i modele generatywne}

\textit{Deep Learning} to dział uczenia maszynowego zajmujący się uczeniem głębokich sieci neuronowych. Zgodnie z twierdzeniem o uniwersalnej aproksymowalności (TODO przypis p194 mlp), sieć o jednej warstwie ukrytej jest w stanie przybliżyć dowolną funkcję borelowsko mierzalną z dowolnie małym błędem, pod warunkiem, że ma wystarczającą liczbę ukrytych neuronów. W praktyce badania wykazały, że sieci głębsze (więcej warstw), nie szersze (więcej neuronów w warstwie), są w stanie przy tej samej objętości osiągać lepsze przybliżenia w wielu różnych dziedzinach. Koncepcyjnie rzecz biorąc, sieć o większej ilości warstw reprezentuje założenie, że szukana aproksymacja będzie składała się z wielu kroków budujących na sobie. Wyniki w dziedzinie przetwarzania obrazu czy języka naturalnego pokazują, że głębokie sieci na przestrzeni swoich warstw rzeczywiście budują coraz bardziej złożone reprezentacje wejścia, budując abstrakcyjne koncepcje z bardziej podstawowych cech. (TODO transformer circuits? that blogpost work illustrating convnets?)

W klasycznym ujęciu, sieci neuronowe uczą się funkcji $Y = f(X)$ przybliżającej rozkład warunkowy $P(Y \mid X)$, gdzie $X$ to dane na wejściu do sieci (na przykład obrazki) a $Y$ to wyjście (na przykład kategoria obiektu na obrazku). Dział modeli generatywnych zajmuje się innym problemem: modelowaniem samego rozkładu $P(X)$ (lub łącznego $P(X, Y)$). W przeciwieństwie do rozkładu warunkowego, pozwala to na generowanie syntetycznych przykładów losując $x \sim P(X)$, skąd bierze się ich nazwa. Paradygmat generatywny ma szeroki zakres zastosowań, takich jak na przykład:
\begin{itemize}
    \setlength\itemsep{0em}
    \item odszumianie obrazu,
    \item detekcja anomalii,
    \item projektowanie leków,
    \item efektywne symulacje fizyczne, oraz
    \item kompresja.
\end{itemize}
Najbardziej powszechnymi architekturami pozwalającymi na modelowanie generatywne są:
\begin{itemize}
    \setlength\itemsep{0em}
    \item GANy (\textit{Generative Adversarial Networks}),
    \item VAE (\textit{Variational Auto-Encoder}),
    \item modele dyfuzyjne, oraz
    \item transformery.
\end{itemize}
Dwie z nich zostaną w tej pracy opisane bardziej szczegółowo, jako że są to główne metody stosowane przy generowaniu dźwięku: VAE oraz GANy. Obie opierają się na sieci zwanej \textit{generatorem}, która przetwarza wektor z nisko-wymiarowej przestrzeni losowej w obiekt z docelowej przestrzeni.

\subsection{VAE}

\textit{Variational Auto-Encodery} parują generator $gen$ z enkoderem $enc$, który odwraca jego działanie i przekształca przykłady do nisko-wymiarowej reprezentacji $z = enc(x)$, nazywaną \textbf{reprezentacją ukrytą} albo wektorem z \textbf{przestrzeni ukrytej} $Z$. Przestrzeń ta ma zadany rozkład \textit{prior} $p(z)$, a generowanie nowych przykładów odbywa się w dwóch krokach: wylosowanie $z \sim p(z)$, a następnie przekształcenie $\hat{x} = dec(z)$.

O takim modelu można myśleć jak o sieci bayesowskiej $z \to x$, gdzie $p(x) = \int p(x \mid z) \, p(z) \, dz$, $z$ jest niezaobserowaną zmienną a $p(x \mid z)$ wraz z $p(z)$ są rozkładami parametryzowanymi. Dla zaobserwowanych danych $X$, nauczenie sieci to kwestia wyprowadzenia optymalnych parametrów. Ponieważ zarówno wartości $z$ jak i parametry rozkładów są nieznane, znalezienie ich za pomocą metody największej wiarygodności jest niemożliwe. Algorytm \textit{expectation-maximization} (EM) pozwala w takiej sytuacji znaleźć lokalnie optymalne rozwiązanie poprzez iteracyjną procedurę poprawiania błędu. Jedna iteracja składa się z dwóch kroków: 
\begin{enumerate}
    \setlength\itemsep{0em}
    \item obliczenie rozkładu \textit{posterior} $p(z \mid x)$ na bazie obecnych parametrów, a następnie
    \item maksymalizacja parametrów względem obecnej estymacji rozkładu posterior.
\end{enumerate}
W przypadu jednak działania na danych typu dźwięk czy obraz i na rozkładach wyrażanych przez sieci neuronowe, pierwszy krok jest często niemożliwy do zrealizowania.

W swojej pracy \textit{Auto-Encoding Variational Bayes}, autorzy wprowadzili algorytm \textit{Stochastic Gradient Variational Bayes} (SVGB) do optymalizacji parametrów w takiej sieci w przypadku, gdy:
\begin{enumerate}
    \setlength\itemsep{0em}
    \item obliczenie $p(z \mid x)$ jest niemożliwe, oraz
    \item zbiór danych jest za duży by działać na całym naraz.
\end{enumerate}
Do tego celu wprowadzają \textit{approximate posterior} $q_{\phi}(z \mid x)$, będący parametryzowanym rozkładem mającym przybliżyć prawdziwy posterior. W ogólności, metody inferencji w sieciach bayesowskich korzystające z jakiegoś rodzaju przybliżenia rozkładu posterior nazywa się metodami inferencji \textit{wariacyjnej} (ang. \textit{variational inference}). Za pomocą takiego przybliżenia da się wyprowadzić dolną granicę na $p(x)$, zwaną \textit{Evidence Lower Bound} (ELBO):
$$p(x) > \mathcal{L}(q, x) = -D_{KL}( \, q_{\phi}(z \mid x) \, \| \, p(z)) + \mathbb{E}_{q}[\log p(x \mid z)]$$
Optymalizując tę granicę można przybliżyć metodę największej wiarygodności. Wymaga to policzenia pochodnych ELBO także względem parametrów $\phi$, co w ogólności robi się zwykle naiwnym estymatorem Monte Carlo. Ten cechuje się jednak bardzo dużą wariancją, co czyni go niepraktycznym. Nakładając pewne niewielkie ograniczenia na \textit{approximate posterior} - przeformułowywując go na jakąś różniczkowalną funkcję $g_{\phi}(\epsilon, \, x)$ gdzie $\epsilon$ to losowy szum ze znanego rozkładu - autorzy wyprowadzają SVGB, alternatywny estymator ELBO:
$$
\widetilde{\mathcal{L}}(q, x) = -D_{KL}(q_{\phi}(z \mid x) \, \| \, p(z)) + \frac{1}{L} \sum\limits_{l=1}^{L} \log p(x \mid z_l)
$$
gdzie $z_l = g_{\phi}(\epsilon_l, \, x)$ i $\epsilon_l \sim p(\epsilon)$. Ponieważ $g_{\phi}(\epsilon_l, \, x)$ jest różniczkowalne, można go optymalizować standardowymi metodami spadku wzdłuż gradientu. W praktyce $L$ może być równe 1 jeśli liczba przykładów, na których SVGB jest estymowane jest wystarczająco duża - autorzy podają przykładowo 100. 

W kontekście auto-enkoderów, rozkłady $q_{\phi}(z \mid x)$ i $p(x \mid z)$ są implementowane przez sieci neuronowe - odpowiednio enkoder i dekoder - a prior $p(z)$ jest zwykle ustalonym rozkładem, takim jak na przykład $\mathcal{N}(0, \, 1)$. Druga część sumy w estymatorze SVGB, $\log p(x \mid z_l)$, odpowiada \textit{błędowi rekonstrukcji} danego przykładu $x$ po przejściu przez enkoder i dekoder, zaś pierwsza część sumy regularyzuje enkoder tak, żeby zadany przez niego rozkład był jak najbliższy $p(z)$.

(TODO i co dalej? współcześnie - podmiana reconstruction, nie do końca ELBO? własności VAE? mode covering, zastosowania? historia może)

\subsection{GANy}

Sieci \textit{Generative Adversarial Networks} parują generator $G_{\theta}$ z dyskryminatorem $D_{\phi}$ - siecią, której zadaniem jest odróżniać prawdziwe od syntetycznych przykładów. Zadaniem generatora jest przekształcić wektor losowego szumu $z \sim p(z)$ w syntetyczny przykład $\hat{x} = G(z)$. Dyskryminator zaś określa prawdopodobieństwo, że dany przykład $x$ jest prawdziwy, i.e. pochodzi z prawdziwego rozkładu danych, nie z tego zadanego przez $G$. Uczenie obu sieci odbywa się poprzez funkcję $v(\theta, \, \phi)$ określającą jak dobrze radzi sobie dyskryminator. Jego celem jest maksymalizacja tej funkcji, zaś celem generatora jest maksymalizacja $-v(\theta, \, \phi)$. Procesie uczenia staje się grą o sumie zerowej, gdzie obie sieci działają w przeciwnych kierunkach. Takie sformułowanie zmusza generator do jak najwierniejszego odtwarzania prawdziwego rozkładu $p(x)$. Oryginalna praca \textit{Generative Adversarial Nets} z 2014 definiuje $v$ jako:
$$v(\theta, \, \phi) = \mathbb{E}_{\mathbf{x} \sim data} \log D(\boldsymbol{x}) + \mathbb{E}_{\boldsymbol{x} \sim G} \log\left(1 - D(\boldsymbol{x})\right).$$
Dynamika uczenia GANów jest zasadniczo inna niż przy standardowych sieciach neuronowych. W praktyce, przy prawdziwych danych oraz $D$, $G$ będącymi głębokimi sieciami, trening rzadko zbiega do stanu ekwilibrium. Funkcje celu oscylują, przez co ciężko jest o łatwe określenie, na jakim etapie trening ten się znajduje. Efektywne uczenie wymaga znalezienia takiego balansu hiperparametrów (prędkość uczenia, liczba kroków dyskryminatora versus jeden krok generatora), żeby ani generator ani dyskryminator nie zdominowały przeciwnika. Mimo tych trudności, dobrze skalibrowane GANy wykazały się bardzo realistycznymi rekonstrukcjami w dziedzinie przetwarzania obrazu. W kolejnych latach powstało wiele wariacji GANów przeformułowywujących funkcję celu lub modyfikujących architektury $D$ i $G$, osiągając przy tym najlepsze wyniki wśród ówczesnych modeli generatywnych.

Istotną modyfikacją w funkcji celu, używaną w wielu pracach związanych z generowaniem dźwięku, było wprowadzenie koncepcji \textit{feature matching loss} (FM). W pracy \textit{Improved Techniques for Training GANs}, autorzy przedstawiają to jako sposób na stabilizację treningu. Zamiast minimalizować czy maksymalizować wyjście dyskryminatora, nowym celem staje się minimalizacja różnicy w jego \textit{wewnętrznych} aktywacjach między prawdziwymi i syntetycznymi przykładami. Koncepcyjnie można myśleć o tym jak o percepcyjnej funkcji straty, gdzie cechy brane pod uwagę są zadane przez dyskryminator. Poprzez manipulację jego architekturą można w bardzo elastyczny sposób kierować tym, jakie cechy będą istotne a jakie nie, co jest powszechnie wykorzystywane w pracach generujących dźwięk i wykorzystujących adwersaryjny trening. Inne prace przedstawiają tę technikę w świetle nie tyle stabilizacji treningu, co wzmocnienia sygnału który generator dostaje od dyskryminatora.

Specyficznym ograniczeniem GANów, wynikającym ze sformułowania celu, jest możliwość zupełnego pominięcia niektórych obszarów $p(x)$ w optymalnym rozwiązaniu. W literaturze takie zachowanie określa się mianem \textit{mode-seeking} - koncentrowanie się na wybranych modach rozkładu - a w patologicznych przypadkach może prowadzić do zjawiska \textit{mode collapse}, gdzie generator uczy się generować pojedyńczy, bardzo realistyczny przykład, ignorując wektor szumu na wejściu. Odróżnia to GANy od metod przybliżających maksymalizacje wiarygodności, jak na przykład VAE, które naturalnie wymagają żeby optymalne rozwiązanie przypisywało każdemu przykładowi treningowemu wysokie prawdopodobieństwo. Niektóre prace starały się zwalczać to zjawisko poprzez dodatkowe składniki funkcji straty penalizujące małą różnorodność wśród wyjść z generatora.

\section*{PyTorch}

Język programowania Python, w związku ze swoją prostą i ekspresywną składnią oraz bogatym ekosystemem wewnętrznych i zewnętrznych bibliotek, stał się de facto standardem w dziedzinie sztucznej inteligencji. Do implementacji sieci neuronowych jedną z najpopularniejszych paczek jest PyTorch, która umożliwia łatwe definiowanie grafów obliczeń na tensorach i automatyczne liczenie w nich pochodnych. Na podstawowych operacjach na tensorach nabudowywuje wiele warstw abstrakcji, takich jak na przykład warstwę konwolucyjną, które pozwalają na definiowanie podstawowych sieci neuronowych w kilku linijkach. Zawiera również implementacje wielu standardowych procedur używanych przy uczeniu maszynowym: funkcje straty, algorytmy optymalizacji wag, klasy transformujące i ładujące dane czy funkcje eksportujące wytrenowane sieci w uniwersalnych formatach.

Kolejną warstwą abstrakcji zbudowaną na PyTorch jest paczka Lightning. Idzie o krok dalej i opakowywuje w klasy funkcjonalności takie jak:
\begin{itemize}
    \setlength\itemsep{0em}
    \item standardowe pętle trenowania i ewaluacji,
    \item zapisywanie i wznawianie treningów,
    \item profilowanie sieci, czy
    \item rozpraszanie treningu na wiele urządzeń.
\end{itemize}

\section*{Normalizing Flows}

\textit{Normalizing Flows} (NF) to metoda elastycznego konstruowania rozkładów prawdopodobieństwa poprzez serię odwracalnych, różniczkowalnych transformacji. Dla transformacji $T$ i zmiennej losowej $u \sim p(u)$ z jakiegoś znanego rozkładu (przeważnie $\mathcal{N}(0, \, 1)$), wynikowy rozkład jest zdefiniowany przez $$x = T(u).$$Prawdopodobieństwo danego $x$ jest wtedy równe $$p(x) = p_u(u) \, |\text{det} J_T(u)|^{-1}, \;\;\; \text{gdzie} \;\;\; u = T^{-1}(x).$$Dowolność transformacji $T$ pozwala wyrazić w ten sposób bardzo bogatą klasę rozkładów, przez co NF naturalnie pasują do zadania przybliżania nieznanych, wysoce złożonych rozkładów. Jest ona zwykle złożona z wielu mniejszych kroków (stąd nazwa \textit{flows}), a jej różniczkowalność pozwala na rozwiązywanie problemu przybliżenia poprzez estymację największej wiarygodności z optymalizacją gradientową. Jest to dokładnie ten sam problem, który rozwiązują modele generatywne, przez co o NF można myśleć jak o kolejnej ogólnej klasie modeli z tej rodziny.

Istotnym kierunkiem badań dla \textit{Normalizing Flows} było znalezienie sposobów na zwiększenie ekspresywności $T$. Praca \textit{Non-linear Independent Component Estimation} (NICE) z 2015 roku opracowała sposób na zdefiniowanie transformacji za pomocą dowolnych sieci neuronowych, które same w sobie nie są odwracalne, pod nazwą \textit{coupling layers}. Celem było stworzenie odwracalnej, różniczkowalnej transformacji parametryzowanej przez dowolną sieć, której wyznacznik macierzy Jacobiego $|\text{det}J_T(u)|^{-1}$ jest łatwo obliczalny w praktyce. Na schemacie (TODO schemat) przedstawiona jest jedna taka warstwa stosująca \textit{additive coupling}. Ponieważ $y_2 = x_2$, operacja $y_1 = x_1 + m(x_2)$ jest trywialnie odwracalna, dając $x_1 = y_1 - m(y_2)$. Dodatkowo, wyznacznik macierzy Jacobiego dla tak zdefiniowanej transformacji jest zawsze równy 1. Aplikując tę architekturę do problemu generowania obrazków, autorzy wykazali, że jest ona w stanie osiągać wyniki porównywalne z innymi generatywnymi metodami na skomplikowanych rozkładach prawdziwych danych.

Praca RealNVP z 2017 roku pociągnęła ten pomysł dalej, wprowadzając szereg usprawnień pozwalających na głębsze architektury NF dostosowane do generowania obrazu. (TODO troche więcej o tym probs, skoro używam defaultowej architektury)

\section*{SeFa}