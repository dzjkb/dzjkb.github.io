## VCTK words
so, some stats:
- original words: 327661
- after filtering: 250620
- train/val: 225749/24872
## Percussive
## SFX

---
# tekst

Niestety, w kontekście badań nad głębokimi sieciami neuronowymi, brakuje otwartych zestawów danych które zbierałyby krótkie, instrumentalne lub perkusyjne brzmienia. Istniejące zestawy mają często inną charakterstykę, zależnie od celu danej pracy:
\begin{itemize}
    \setlength\itemsep{0em}
    \item ciągły dźwięk - prace takie jak RAVE, MelGAN czy RVQGAN przetwarzają ciągłe strumienie audio zamiast pojedyńczych klipów. Korzystają z zestawów takich jak VCTK czy Emilia (nagrania głosu), nagrań instrumentalnych performensów, lub innych niemuzycznych dźwięków;
    \item instrumentalne - prace takie jak GANSynth czy SING badają krótkie instrumentalne klipy, jednak o ograniczonej różnorodności w dynamice czy barwie;
    \item całe utwory - prace takie jak Jukebox czy MusicLM przetwarzają całe wyprodukowane utwory.
    \item losowe nagrania - prace takie jak CLAP, których celem jest stworzenie unierwsalnych embeddingów, w swoim zestawie łączą bardzo dużą 
\end{itemize}
Najbliższym kandydatem do potrzeb tej pracy byłby zestaw wykorzystany w DrumGAN a potem CRASH, jednak autorzy nie zdecydowali się go upublicznić. W związku z tym, rozwiązanie jest ewaluowane na trzech własnoręcznie przygotowanych zestawach: \textit{VCTK words}, \textit{Percussive} i \textit{SFX}.

\section*{VCTK words}

\textit{VCTK words} to zestaw przygotowany na bazie \textit{Voice Cloning ToolKit} (VCTK). VCTK składa się z około 44 godzin nagrań w studyjnej jakości, w których 110 ludzi czyta około 400 zdań po każdy, po angielsku, wraz z transkrypcją. \textit{VCTK words} powstało poprzez wycięcie z nich pojedyńczych słów. O ile nie są to muzyczne dźwięki, mają kilka cech które dobrze wpasowywują się do tej pracy:
\begin{itemize}
    \setlength\itemsep{0em}
    \item są krótkie (poniżej 1 sekundy) i zróżnicowane w dynamice i barwie, z dużą ilością detali;
    \item pozwalają na bezpośrednie porównanie z innymi pracami które robiły ewaluację na VCTK, jak na przykład RAVE;
    \item dają się opisać poprzez zestaw wysokopoziomowych cech: wypowiedziane słowo, osoba mówiąca, płeć, akcent, co można wykorzystać w ewaluacji przestrzeni ukrytej.
\end{itemize}
Ponieważ transkrypcje nagrań nie mają oznaczeń czasowych dla pojedyńczych słów, pierwszym krokiem w przygotowaniu \textit{VCTK words} było stworzenie tego dopasowania. W tym celu użyty został model \textbf{Qwen3-ForcedAligner-0.6B} z serii modeli Qwen do zadań wokół rozpoznawania mowy, wypuszczony w tym roku. Żeby usunąć ciężkie przypadki - albo błędne, takie w których słowo praktycznie nie jest rozpoznawalne - zostały one następnie przefiltrowane do długości między 0.1 a 1 sekundy. W naturalnej, szybkiej mowie słowa prawie nakładają się na siebie, więc zdarzały się przypadki, w których takie jak na przykład "a" skutkowały wycinkiem poniżej 0.1s, który nie miałoby sensu modelować. Słowa powyżej 1 sekundy albo zawierały ciszę albo rzeczywiście były długie, jednak wydłużenie całego zestawu danych do najdłuższego z nich wprowadziłoby za dużo ciszy do większości przykładów. Proces filtrowania zmniejszył rozmiar zbioru z 327661 do 250620 przykładów o całkowitej długości 22 godzin 33 minut. Tabelka~\ref{tab:vctk-statystyki} przedstawiają rozkład różnych metadanych w końcowym zestawie.

\begin{table}[htbp]
    \centering
    \caption{Statystyki nt. zbioru VCTK words}
    \label{tab:vctk-all}
    \vspace{4pt}
    \begin{subtable}[t]{0.48\textwidth}
        \centering
        \caption{płeć}
        \label{tab:vctk-gender}
        \vspace{4pt}
        \begin{tabular}{lc}
            \toprule
            \textbf{grupa} & \textbf{przykłady} & \textbf{mówcy} \\
            \midrule
            łącznie & 250620 & 109 \\
            żeńskie & 146261 & 63 \\
            męskie & 104359 & 46 \\
            \midrule[\heavyrulewidth]
            angielski & 76585 & 33 \\
            amerykański & 47277 & 21 \\
            szkocki & 43936 & 19 \\
            irlandzki & 19850 & 9 \\
            kanadyjski & 17632 & 8 \\
            północno-irlandzki & 14073 & 6 \\
            południowo-afrykański & 9764 & 4 \\
            indyjski & 6757 & 3 \\
            australijski & 4465 & 2 \\
            brytyjski & 2927 & 1 \\
            nieznany & 2769 & 1 \\
            nowozelandzki & 2478 & 1 \\
            walijski & 2107 & 1 \\
            \bottomrule
        \end{tabular}
    \end{subtable}
    \hfill
    \begin{subtable}[t]{0.48\textwidth}
        \centering
        \caption{najpopularniejsze słowa}
        \label{tab:vctk-top-words}
        \vspace{4pt}
        \begin{tabular}{lc}
            \toprule
            \textbf{słowo} & \textbf{liczebność} \\
            \midrule
            was & 5538 \\
            is & 5538 \\
            the & 5538 \\
            it & 5538 \\
            not & 5538 \\
            that & 5538 \\
            in & 5538 \\
            this & 5538 \\
            for & 5538 \\
            i & 5538 \\
            of & 5538 \\
            we & 5538 \\
            have & 5538 \\
            he & 5538 \\
            and & 5538 \\
            \bottomrule
        \end{tabular}
    \end{subtable}
\end{table}

\section*{Percussive}

O ile nie ma publicznie dostępnych perkusyjnych zestawów danych pod uczenie maszynowe, w internecie jest ogromna ilość \textit{paczek sampli} różnego rodzaju - zbiorów brzmień stworzonych przez kogoś pod kątem produkcji muzyki. Zwykle są jednak one o wiele mniejsze niż zestawy do Deep Learning'u. Te w wysokiej jakości, profesjonalnie produkowane, są często płatne, lecz istnieje też dużo darmowych paczek w których jakość jest wystarczająco wysoka. Zbiór \textit{Percussive} złożony został z połączenia kilku takich paczek wraz z automatycznie przefiltrowanym podzbiorem \textit{LAION-audio-630K}. Zestaw \textit{LAION-audio-630k} złożony jest z kilku części, z czego jedną z nich jest 515581 sampli ściągniętych z serwisu Freesound \footnote{https://freesound.org}, każdy z tytułem i opisem. Na bazie połączenia tytułu i opisu, używając modelu językowego \textit{Qwen2.5-14B}, przefiltrowane zostały one do 8560 przykładów perkusyjnych podobnych do werbli. Niestety większość przykładów nie zawierały dodatkowych metadanych, pozwalających określić na przykład kategorię instrumentu. Niewielki podzbiór \textit{Percussive} ma przypisaną kategorię na podstawie nazwy ściągniętego pliku, jeśli przykładowo zawierała ona w sobie słowo "kick" - oznacza to, że sample jest stopą perkusyjną. Przykłady w \textit{Percussive} są niedłuższe niż 1 sekunda. Tabelka~\ref{tab:perc-zrodla} przedstawia rozkład przykładów po źródłach lub kategoriach.

\begin{table}[htbp]
    \centering
    \caption{Statystyki nt. zbioru Percussive}
    \label{tab:perc-zrodla}
    \vspace{4pt}
    \begin{tabular}{lc}
        \toprule
        \textbf{pozdbiór} & \textbf{liczebność} \\
        \midrule
        łącznie & 14963 \\
        freesound & 8560 \\
        paczki sampli & 6403 \\
        \midrule[\heavyrulewidth]
        nieznana kategoria & 10950 \\
        snare & 1856 \\
        kick & 1529 \\
        hat & 354 \\
        cymbal & 141 \\
        clap & 133 \\
        \bottomrule
    \end{tabular}
\end{table}

\section*{SFX}

Zestaw \textit{SFX}, od angielskiego \textit{sound effects}, jest mocno zróżnicowanym zbiorem abstrakcyjnych dźwięków. Podobnie jak przy \textit{Percussive}, powstał poprzez przefiltrowanie podzbioru Freesound za pomocą modelu językowego \textit{Qwen2.5-14B} z odpowiednią kwerendą, dając w ten sposób 57288 przykładów. Przykłady te zostały następnie przefiltrowane według długości oraz kilku cech obliczonych za pomocą narzędzia \textit{essentia} - \textit{dynamicznej złożoności}, \textit{wyrazistość wysokości dźwięku}, \textit{średnia głośność}, \textit{płaskość obwiedni} oraz \textit{stosunku parzystych/nieparzystych harmonicznych} - żeby bardziej ujednolicić charakter zbioru i usunąć przykłady potencjalnie niskiej jakości. W skutek tej procedury powstał zbiór 19696 przykładów o maksymalnej długości 1.6 sekundy. Składa się on z ciężkich do zaklasyfikowania, syntetycznych lub naturalnych dźwięków, nagrań, efektów czy hałasów.