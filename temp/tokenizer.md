---
draft: true
---
https://vinija.ai/nlp/tokenizer/


https://github.com/google/sentencepiece


## Enriching Word Vectors with Subword Information

Continuous  word  representations,  trained  on large  unlabeled  corpora  are  useful  for  many natural  language  processing  tasks.    Popular models that learn such representations ignore the morphology of words, by assigning a dis-tinct vector to each word. This is a limitation,especially for languages with large vocabularies and many rare words.

Considering Unigram word model they are higher chances of getting rare words during testing because we can't take infinite size corpous which includes all the words.

Here each word is represented as a bag of character n-grams. A vector representation of each character n-gram is summed to get word vector.

By representing each word as a bag of character n-grams a SkipGram model with Negative sampling is trained.

By  using  a  distinct  vector  representation  for  each word, the skipgram model ignores the internal structure of words.

![](images/subword_1.png)
![](images/subword_2.png)

Hyperparameter choice for generating Fasttext embeddings

1. Generating fasttext embedding will take more time compared to word2vec model.
2. As the corpus size grows, the memory requirement grows too - the number of ngrams that get hashed into the same ngram bucket would grow. So the choice of hyperparameter controlling the total hash buckets including the n-gram min and max size have a bearing.
