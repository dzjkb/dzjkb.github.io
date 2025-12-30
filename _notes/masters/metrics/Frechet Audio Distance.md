reference-free audio quality metric
[original paper](https://arxiv.org/abs/1812.08466)
[google research impl](https://github.com/google-research/google-research/tree/master/frechet_audio_distance)
- tensorflow though :c

[lightweight pytorch lib](https://github.com/gudgud96/frechet-audio-distance) available in PyPI
- seems to be maintained, nice
- has EnCodec embeddings available, probably the most up-to-date model?

oh shit, [FAD toolkit](https://github.com/microsoft/fadtk) from Microsoft
- **!! Adapts FAD to generative music evaluation** - [paper](https://arxiv.org/abs/2311.01616)
    - tf
    - MusicCaps dataset + 3 reference datasets
    - uses GPT-4 to extract quality labels from MusicCaps and then compares FAD output with these
- has EnCodec but also Whisper and [[RVQGAN]]! available
- also pytorch
- [online demo](https://fadtk.hydev.org/)
paper takeaways
- FAD measures different things depending on reference set + embeddings
- depends on what qualities the embeddings capture + what kind of tracks are in the reference set
- why would I want to compare with just random genre random songs anyway?
- does this make sense if I'm doing single instrument 3-5s samples then? probably not??????? fuck
- unless I use a sample pack as reference + which embeddings?
- can I use the train/test set as the reference set too?
```
When using
MusCC, all five excerpts with the lowest FAD score fall within the
Western music genres Pop and Rock (consistent with the dominant
genres in MusCC), whereas using FMA-Pop the five excerpts span a
variety of genres, including Jazz and Latin music. 
```
- "These anecdotal findings suggest that per-song FAD scores may be a useful tool to find outliers" - outliers => diversity???

who uses FAD again and how do they use it?
- [[musicLM]]
- and that's it????

ok lesgo
- [x] read the Microsoft paper on FAD adaptation, decide on implementation
- decide if we want to use this at all?
- decide on reference set + embeddings