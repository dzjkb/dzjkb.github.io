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
# generating a dataset
```
python -m src.cli dataset-from-file --target_dir <dataset_dir> --target_length 4 --overlap 1 --in_file track.flac
```
~~remember about the required 48kHz sampling rate~~ the script does resampling if necessary
# technicalities
- 1-3 second clips
# other
check this out
https://github.com/LAION-AI/audio-dataset
![[Pasted image 20251209174422.png]]