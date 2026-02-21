[dataset repo](https://github.com/LAION-AI/audio-dataset)
- [laion-audio-630k specific dir](https://github.com/LAION-AI/audio-dataset/tree/main/laion-audio-630k)
[CLAP repo](https://github.com/LAION-AI/CLAP) for reference
[example of dataset format (gdrive)](https://drive.google.com/drive/folders/1scyH43eQAcrBz-5fAw44C6RNBhC3ejvX)

4 datasets available: (**BBC sound effects, Epidemic Sound, Audiostock and Freesound**)
Freesound is available on [huggingface](https://huggingface.co/datasets/Meranti/CLAP_freesound), rest is csvs with urls that you need to download yourself probs?
huggingface has processed datasets in [webdataset](https://github.com/webdataset/webdataset) format
[the Freesound ds card](https://github.com/LAION-AI/audio-dataset/blob/main/data_card/freesound.md)

try find some subset here
https://huggingface.co/datasets?sort=trending&search=laion
- [this](https://huggingface.co/datasets/AghaTizi/Instrumental-LAION-Audio-300M) is an attempt at filtering music, but it's like track snippets, not samples
## Freesound full
695GB jesus
- test 68.9GB
- train ~630GB
- 516093 samples in total (according to the csv, 515581 according to the README)
- 3033.38hrs

each sample has 7 associated columns in the `freesound_meta.csv`:
```
audio_filename,caption1,caption2,freesound_id,username,freesound_url,license
```
no easy way to even do any basic statistics of sample types etc
browse in a notebook
#### filtering
no simple tags to use, try to match words in descriptions?
LLMs? hmmmm
#### download
ok maybe this is doable - download it to either
- a new HDD (e.g. [WD Elements 5TB](https://allegro.pl/oferta/dysk-twardy-western-digital-elements-17742408955))
    - pros: new disk for backups yay
    - cons: 500pln
    - maybe [2TB](https://allegro.pl/oferta/dysk-przenosny-wd-elements-portable-2tb-czarny-18256937558) would be enough? that's 350pln ok the price ratio doesn't make sense
    - fkn offer unavailable, [different offer 30pln more](https://allegro.pl/oferta/dysk-wd-elements-portable-5tb-hdd-czarny-18256933762)
    - or maybe [4TB for 440pln](https://allegro.pl/oferta/dysk-wd-elements-portable-4tb-hdd-18078965146?reco_id=ea20d64a-f922-11f0-a436-763530dff3b3&sid=1147bbb04b955b96a226cab2e664258d9124ef0862320b2d7366fdd6604cf017)
- this old toshiba HDD I have?
    - pros: free
    - cons: only 1TB, very questionable reliability - it's old
- AWS storage
    - 80$/month for 1TB, that's the new HDD price after ~2 months already
ok 4TB arrived let's do this

ok how do I even download this
https://huggingface.co/docs/hub/en/datasets-downloading
https://huggingface.co/docs/huggingface_hub/en/guides/cli#download-a-dataset-or-a-space
need their CLI I guess

right, downloading
`HF_TOKEN=`
ok done : o