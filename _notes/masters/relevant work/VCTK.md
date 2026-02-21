[original paper](https://ieeexplore.ieee.org/document/6709856)
[hugging face](https://huggingface.co/datasets/CSTR-Edinburgh/vctk)
[edinburgh datashare download](https://datashare.ed.ac.uk/handle/10283/3443)
files link: https://datashare.ed.ac.uk/download/DS_10283_3443.zip

44 hours
110 english speakers, each reads around 400 sentences
11GB

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