# Arabic BPE Tokenization Using Google Sentance Piece

Natural Language Processing is a branch of AI. One of the first steps in any NLP system is language model encoding. The challenge is how to present/encode the words efficiently.
Sub-word encoding is very suitable to arabic. For example the word `مدرساتهم` will not be considered a single token/word, but split into three; `مدرس`, `ات`, and `هم`. This is the basic intuition.
This process is done automatically without any rules or preprocessing.

### Motive
I have came across different project with the same intention, but the results were not satisfying.
 - Low quality: https://arxiv.org/pdf/1710.02187v1.pdf seems to be a scripted work without any manual lingual review. 
 - Small datasets: https://aclanthology.org/W19-4602.pdf with focus on arabic dialects not MSA.

## Workflow

### Prerequisites
Tested on: Ubuntu 22.04
```
sudo apt-get install wget xz-utils cmake build-essential pkg-config libgoogle-perftools-dev
```

### Download the Arabic Corpus
```
./arabic_corpus/download_corpus.sh
````
This corpus has no lines, just a large stream of strings with Windows-1256 encoding. Sentence Piece expects to read each sentence on a line with UTF-8 encoding. Thus we need to break the corpus to the max sentence length:
```
fold -s -b -w2096 arabic_corpus/arabic_corpus \
   | head -n 1000000 \
   | iconv -f WINDOWS-1256 -t UTF-8 \
   > arabic_corpus/arabic_corpus_lines
rm -f arabic_corpus/arabic_corpus
```
### Build Sentaince Piece
```
mkdir sentencepiece/build
cd sentencepiece/build
cmake ..
make -j $(nproc)
sudo make install
sudo ldconfig -v
```

### Training on the Arabic Corpus
```
spm_train --input=arabic_corpus/arabic_corpus_lines --model_prefix=ar8k --vocab_size=8000 --character_coverage=1.0 --model_type=bpe
```
