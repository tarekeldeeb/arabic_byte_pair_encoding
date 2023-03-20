# Arabic BPE Tokenization Using Google Sentance Piece

Natural Language Processing is a branch of AI. One of the first steps in any NLP system is language model encoding. The challenge is how to present/encode the words efficiently.
Sub-word encoding is very suitable to arabic. For example the word `مدرساتهم` will not be considered a single token/word, but split into three; `مدرس`, `ات`, and `هم`. This is the basic intuition.
This process is done automatically without any rules or preprocessing.

## Prerequisites
Tested on: Ubuntu 22.04
```
sudo apt-get install wget xz cmake build-essential pkg-config libgoogle-perftools-dev
```

## Download the Arabic Corpus
```
./arabic_corpus/download_corpus.sh
````

## Build Sentaince Piece
```
mkdir sentencepiece/build
cd sentencepiece/build
cmake ..
make -j $(nproc)
sudo make install
sudo ldconfig -v
```

## Training on the Arabic Corpus
```
TBD ..
```
