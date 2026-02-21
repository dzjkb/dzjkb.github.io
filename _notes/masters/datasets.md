located in [gdrive](https://drive.google.com/drive/folders/1tsrUCRlhRJL-kpXTx3pIz1TGkdRdDss7) and on `gpu2`
random sample packs from the internet, chosen albums
I love soulseek
research on copyrighted works should be OK right?
# listed
## snares
(done)
random sample packs from the internet
## peripeteia
(done)
## graz
(done)
## debussy
(done)
just preludes
## chopin
(done)
scherzos + ballads should be enough - Zimmerman
or even just scherzos, 40+ min more than enough
## soft channel
(done)
oh shit
https://giantclaw.bandcamp.com/album/soft-channel
## sfx
## custom synth sounds
how long would it take me to produce like an hour worth of specific synth samples
what synth samples would this be?
- 
## loops
## FSDKaggle2018
https://zenodo.org/records/2552860
- "If you use the FSDKaggle2018 dataset or part of it, please cite our [**DCASE 2018 paper**](https://arxiv.org/abs/1807.09902):"
- [kaggle site](https://www.kaggle.com/competitions/freesound-audio-tagging)
```
is an audio dataset containing 11,073 audio files annotated with 41 labels of the [AudioSet Ontology](https://research.google.com/audioset////////ontology/index.html)

"Acoustic_guitar", "Applause", "Bark", "Bass_drum", "Burping_or_eructation", "Bus", "Cello", "Chime", "Clarinet", "Computer_keyboard", "Cough", "Cowbell", "Double_bass", "Drawer_open_or_close", "Electric_piano", "Fart", "Finger_snapping", "Fireworks", "Flute", "Glockenspiel", "Gong", "Gunshot_or_gunfire", "Harmonica", "Hi-hat", "Keys_jangling", "Knock", "Laughter", "Meow", "Microwave_oven", "Oboe", "Saxophone", "Scissors", "Shatter", "Snare_drum", "Squeak", "Tambourine", "Tearing", "Telephone", "Trumpet", "Violin_or_fiddle", "Writing".
```
## VCTK
[[VCTK]]
# generating a dataset
```
python -m src.cli dataset-from-file --target_dir <dataset_dir> --target_length 4 --overlap 1 --in_file track.flac
```
~~remember about the required 48kHz sampling rate~~ the script does resampling if necessary
# technicalities
- 1-3 second clips
# other
check this out
![[Pasted image 20251209174422.png]]
[[LAION-audio-630K]]