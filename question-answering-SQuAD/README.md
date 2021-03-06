# Qusetion Answering system for SQUAD
This repo contains a simple source code for building question-answering system for SQuAD.

## Data
Stanford Question Answering Dataset (SQuAD) is a reading comprehension dataset, consisting of questions posed by crowdworkers on a set of Wikipedia articles, where the answer to every question is a segment of text, or span, from the corresponding reading passage, or the question might be unanswerable.
You can download this dataset [here](https://rajpurkar.github.io/SQuAD-explorer/).

- `SQuAD 1.1`: the previous version of the SQuAD dataset, contains 100,000+ question-answer pairs on 500+ articles.
- `SQuAD 2.0`: combines the 100,000 questions in SQuAD 1.1 with over 50,000 new, unanswerable questions written adversarially by crowdworkers to look similar to answerable ones.

Here I used SQuAD 1.1. Each article contains following structure:
```
structure:
  article (dict)
  ├── title (str)
  ├── paragraphs (dict)
      ├── context (str)
      └── qas (dict)        
          ├── question (str)
          ├── id (str)
          └── answers (dict)
              ├── answer_start (integer)
              └── text (str)
  ...
  └── paragraphs n
      ├── context
      └── qas 
```
sample:
```
'title': 'University_of_Notre_Dame'
'paragraphs': [{
  'context': 'Architecturally, the school has a Catholic character. Atop the Main Building\'s gold dome is a golden statue of the Virgin Mary. Immediately in front of the Main Building and facing it, is a copper statue of Christ with arms upraised with the legend "Venite Ad Me Omnes". Next to the Main Building is the Basilica of the Sacred Heart. Immediately behind the basilica is the Grotto, a Marian place of prayer and reflection. It is a replica of the grotto at Lourdes, France where the Virgin Mary reputedly appeared to Saint Bernadette Soubirous in 1858. At the end of the main drive (and in a direct line that connects through 3 statues and the Gold Dome), is a simple, modern stone statue of Mary.',
  'qas': [{
    'answers': [{
        'answer_start': 515,
        'text': 'Saint Bernadette Soubirous'
        }],
    'question': 'To whom did the Virgin Mary allegedly appear in 1858 in Lourdes France?',
    'id': '5733be284776f41900661182'
    },
    {
    'answers': [{
        'answer_start': 188,
        'text': 'a copper statue of Christ'
    }],
    'question': 'What is in front of the Notre Dame Main Building?',
    'id': '5733be284776f4190066117f'
    }
  ]},
  {'context': ...., 'qas': ....},
  {'context': ...., 'qas': ....}]

'title': ...
'paragraphs': {...}
```

## Usage
### 1. Preprocessing corpus
First, prepare data. Donwload SQuAD data, GloVe and nltk corpus.

You can adopt the pre-trained word embedding model in the below of your choice. I tested both Glove and Fasttext, and used Glove embedding here.
- `Glove(glove.6B.100d.txt)`:  trained on Wikipedia 2014 + Gigaword 5 (6B tokens, 400K vocab, uncased, 50d, 100d, 200d, & 300d vectors, 822 MB download). 
- `Fasttext(wiki.en.bin or wiki.en.vec)`: trained on Wikipedia (2519370 vocab, 300d vectors).

example usage:
```
chmod +x download_squad.sh; ./download_squad.sh
chmod +x download_glove.sh; ./download_glove.sh
pip install nltk==3.2.5
$ ipython
>>> import nltk
>>> nltk.download()
```

Second, Preprocess SQuAD dataset (along with GloVe vectors) and save them in current working directory:
It will create vocabulary, context-question vectors, and embedding matrix used for MRC model training in order.
```
python preprocessing.py -h
usage: preprocessing.py [-h] [-mode MODE] [-glove_vec_size GLOVE_VEC_SIZE]

optional arguments:
  -h, --help            show this help message and exit
  -mode MODE            select train or dev
  -glove_vec_size GLOVE_VEC_SIZE
                        embedding vector size
```

example usage:
```
python preprocessing.py -mode train -glove_vec_size 100
```


## Reference
### Word embeddings
- [GloVe: Global Vectors for Word Representation](https://nlp.stanford.edu/projects/glove/)
- [facebookresearch/fastText](https://github.com/facebookresearch/fastText)
### MRC models
- [allenai/bi-att-flow](https://github.com/allenai/bi-att-flow)
- [Rahulrt7/Machine-comprehension-Keras](https://github.com/Rahulrt7/Machine-comprehension-Keras)
- [rajpurkar/SQuAD-explorer](https://github.com/rajpurkar/SQuAD-explorer)
