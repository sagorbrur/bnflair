# BNFLAIR
A [Flair](https://github.com/flairNLP/flair) based Bengali collections which provide different bengali flair embeddings and Bengali flair trained NER, POS, Text classification model.

## Installation
```
pip install -r requirements.txt
```

## Embeddings
### Bengali Wiki Flair embeddings
Here we have trained Flair character based language model for Bengali Wiki dataset.

- [Forward LM](https://github.com/sagorbrur/bnflair/tree/main/models/embeddings/wikipedia)
    - Total wikipedia artcles: 110449
    - Train epoch: 5 Epochs
    - Validation loss: 1.5366
    - Validation perplexity: 4.6490
    
- [Backward LM](https://github.com/sagorbrur/bnflair/tree/main/models/embeddings/wikipedia)
    - Total wikipedia artcles: 110449
    - Train epoch: 5 Epochs
    - Validation loss: 1.4717
    - Validation perplexity: 4.3566

## Bengali NER Model
### Wikiann Model
Here we have trained Bengali NER model for [wikiann](https://huggingface.co/datasets/wikiann) Bengali NER dataset.

- Total wikiann train data: 1000
- Total wikiann validation data: 100
- TOTAL wikiann test data: 100
- Train epoch: 70 Epochs
- Score in Test data
    - F-score (micro) 0.7751
    - F-score (macro) 0.775
    - Accuracy 0.7364
- For details log check [here](https://github.com/sagorbrur/bnflair/tree/main/models/ner)

## Usage
### Embeddings
- To generate flair embedding using any Bengali text

```py
from flair.data import Sentence

sentence = Sentence('রামপ্রসাদ সেন জন্মগ্রহণ করেছিলেন গাঙ্গেয় পশ্চিমবঙ্গের এক তান্ত্রিক বৈদ্যব্রাহ্মণ পরিবারে।')

# init embeddings from your trained LM
char_lm_embeddings = FlairEmbeddings('bnflair/models/embeddings/wikipedia/bnwiki_backward.pt')

# embed sentence
char_lm_embeddings.embed(sentence)

```

- To fine-tune for training flair based NER, POS, Text classification model

```py
from flair.embeddings import StackedEmbeddings

embedding_types = [
    FlairEmbeddings('bnflair/models/embeddings/wikipedia/bnwiki_forward.pt'),
    FlairEmbeddings('bnflair/models/embeddings/wikipedia/bnwiki_forward.pt')
]

embeddings = StackedEmbeddings(embeddings=embedding_types)

```

### NER
- To use NER model
```py
from flair.data import Sentence
from flair.models import SequenceTagger

text = "কবিরঞ্জন রামপ্রসাদ সেন (১৭১৮ বা ১৭২৩ – ১৭৭৫) ছিলেন অষ্টাদশ শতাব্দীর এক বিশিষ্ট বাঙালি শাক্ত কবি ও সাধক।"
ner_model = "bnflair/models/ner/wikiann.pt"

sentence = Sentence(text)
ner_model.predict(sentence)
entities = sentence.get_spans('ner')

for entity in entities:
    print(entity)

# output: Span[0:3]: "কবিরঞ্জন রামপ্রসাদ সেন" → PER (0.5903)
```