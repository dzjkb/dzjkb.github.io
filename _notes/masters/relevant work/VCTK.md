[original paper](https://ieeexplore.ieee.org/document/6709856)
[hugging face](https://huggingface.co/datasets/CSTR-Edinburgh/vctk)
[edinburgh datashare download](https://datashare.ed.ac.uk/handle/10283/3443)
files link: https://datashare.ed.ac.uk/download/DS_10283_3443.zip

44 hours
110 english speakers, each reads around 400 sentences
11GB
44283 samples

one data point:
```
{
  'speaker_id': 'p225',
  'text_id': '001',
  'text': 'Please call Stella.',
  'age': '23',
  'gender': 'F',
  'accent': 'English',
  'region': 'Southern England',
  'file': '/datasets/downloads/extracted/8ed7dad05dfffdb552a3699777442af8e8ed11e656feb277f35bf9aea448f49e/wav48_silence_trimmed/p225/p225_001_mic1.flac',
  'audio':
    {
      'path': '/datasets/downloads/extracted/8ed7dad05dfffdb552a3699777442af8e8ed11e656feb277f35bf9aea448f49e/wav48_silence_trimmed/p225/p225_001_mic1.flac',
      'array': array([0.00485229, 0.00689697, 0.00619507, ..., 0.00811768, 0.00836182, 0.00854492], dtype=float32),
      'sampling_rate': 48000
    },
  'comment': ''
}
```
# Words dataset
initial brainstorm in [[2026-01-30]]
steps:
- do forced alignment using Qwen3-ForcedAligner-0.6B
- cut out words
- prepare this in webdataset format, upload to huggingface
- run experiments

https://claude.ai/share/998b5e9c-0079-4831-823d-ffd459fc9645 sounds good eh
#### Qwen3 forced aligner
what are the odds this doesn't work with torch<2.0? uses torch 2.10
yeah might be better to run on AWS, should be quick though, DL might take longer
- ok loading/launching the model was instantaneous, idk what's the deal with `flash-attn` f it

pyproject.toml
```
[project]
name = "vctk-env"
version = "1.0.0"
description = "processing vctk"
readme = "README.md"
dependencies = [ 
    "qwen-asr>=0.0.6",
    "rich>=14.3.3",
]
```
#### Dataset exploration
327661 samples
5841 unique words
top words:
```
the 21237
a 11175
is 9244
it 8232
to 7919
was 6756
of 6107
in 5114
i 4374
we 3985
he 3943
that 3683
for 3494
not 3412
have 3344
be 3182
they 2871
and 2784
this 2699
will 2619
```

duration distribution (without a couple of large outliers)
![[Pasted image 20260223182402.png]]
#### Post-processing
1. filter out very short samples
    1. what would be the target length though? "superimposition" does actually last 1.44s
    2. a big majority of the samples fall under 0.8, sounds good
    3. and over what, 0.08s?
    4. ok 0.1-1.0 actually, 0.1 is still very short and like barely not glitchy
2. ~~cut out starting silence~~
    1. `sox`
    2. but the noise floor is high for some reason
    3. ok but this is a tiny part of all samples, I can just filter them out

original location: `/media/Data/jp_dir/sounds/vctk_word_level`
filtered location: `/media/Data/jp_dir/sounds/vctk_words_v2`
## perceptual categories
- speaker identity
- word duration
- gender
- word "type" - ?
- accent - british/american/? labeling each speaker manually seems doable
- (?) phonetic content - some kind of specctral analysis?
- (?) speaking rate - don't think this makes any sense

#### labels csv
- [x] prepare a file id + labels csv for vctk words

there's a `speaker-info.txt` in the original dataset nice