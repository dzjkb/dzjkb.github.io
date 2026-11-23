iiinspiration:
- [[DrumGAN]] abstract
- [[Neural Drum Machine]] abstract

in points:
- advances in AI are also being used for creative audio applications - many types of VSTs, integrations with DAWs or Max/MSP
- one of the directions is neural synthesizers using generative architectures that learn the synthesis process from data rather than having it explicitly designed (which is what AI generally does - replace manual algorithms with learned ones)
- the goal is to create a neural synthesizer of _individual samples_ (not whole tracks) providing _high-level_, _intuitive_ and _global_ controls for sound generation
    - something like a drum-machine
- controls can be manually selected by conditioning the learning process, but we want to learn the controls instead
- VAEs make this combination possible if we fix the clip length and have a single latent vector per audio sample

should I somehow mention what improves over the baselines?
- [[RAVE]] - single vector means global features, fixed length coherent samples
- [[DrumGAN]] - better quality, learned features without conditioning

---

Synteza dźwięku jest kluczowym elementem produkcji muzyki, tradycjnie wykonywanym poprzez analogowe lub cyfrowe narzędzia wykorzystujące techniki syntezy takie jak addytywna, subtraktywna czy granularna. Brzmienie pojedyńczego dźwięku jest kształtowane w pożądany sposób za pomocą różnych parametrów, zależnych od stosowanego rodzaju syntezy. Deep Learning pozwala na alternatywny sposób syntezy dźwięku poprzez wyuczone do tego sieci neuronowe. Korzystając z odpowiedniego rodzaju generatywnych modeli, proces ten da się kontrolować za pomocą zestawu parametrów podobnie do tradycyjnej syntezy, jednak w przeciwieństwie do niej parametry te są abstrakcyjnymi, wysoko-poziomowymi cechami dźwięku aniżeli ciężkimi do zinterpretowania, nisko-poziomowymi własnościami algorytmów syntezy. Parametry te mogą zostać manualnie wyspecyfikowane poprzez warunkowe uczenie, co wymaga dużego nakładu pracy w oznaczaniu danych, albo wyuczone w nienadzorowany sposób z nieoznaczonych danych. W tej pracy próbuję zastosować do tego zadania Variational Auto-Encodery na trzech różnych, własnoręcznie zebranych zestawach danych - angielskich słowach, brzmieniach perkusyjnych oraz efektów specjalnych - wyciągając z nich parametry w nienadzorowany sposób. Pokazuję, że modele te są w stanie generować dźwięk w 48kHz o wysokiej jakości, oraz że modele te uczą się skompresowanej reprezentacji brzmień w której zakodowane są ich wysoko-poziomowe, intuicyjne własności. Modyfikowanie tej reprezentacji w odpowiedni sposób pozwala na kontrolowanie tych parametrów w syntezowanych dźwiękach. Dodatkowo opisuję sposób w jaki wytrenowany model mógłby zostać opakowany we wtyczkę gotową do użycia w dowolnym cyfrowym środowisku do produkcji muzycznej (DAW).