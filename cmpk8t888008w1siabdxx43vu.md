---
title: "Word2Vec Demystified: From One‑Hot to Vector Magic (CBOW & Skip‑Gram Explained)"
datePublished: 2026-05-24T20:41:32.652Z
cuid: cmpk8t888008w1siabdxx43vu
slug: word2vec-demystified-from-one-hot-to-vector-magic-cbow-skip-gram-explained

---

Last article we discussed RAGs; the central idea there is generating embeddings. In this post we'll explain how embedding models work — a small prerequisite is a basic understanding of neural networks. Our goal is to train a model that produces embeddings: vectors in which similar words have high cosine similarity. Today we'll focus on how Word2Vec works and its two training approaches — Continuous Bag-of-Words (CBOW) and Skip‑Gram. Word2Vec yields static embeddings, meaning the same word maps to the same vector across all sentences.

Todays article we will first focus on the working of word2vec and its different techniques and algorithm.  
Word2Vec is a static embedding model. The meaning of static embeddings is for all sentences a particular word will have same embedding vector.  
  
An image from a medium article explains it more clearly:

![](https://cdn.hashnode.com/uploads/covers/6a06127cbaf09db7a628dd57/c6956108-d76f-44ef-ba2e-11961747d3a8.png align="center")

Word2vec is a two layered neural network. There are two algorithms for training.  
1) Continuous Bag Of Words (CBOW)  
2) Skip Grams

![](https://cdn.hashnode.com/uploads/covers/6a06127cbaf09db7a628dd57/df604f65-1ae2-454a-808e-94311d29eec7.png align="center")

First we convert all the words to vector, for that the most frequent and easy used algorithm is one hot encoding.  
One Hot Encoding: Its a way of converting a certain word into vectors. An image from InsightsFromRishi youtube channel explains it very clearly.

![](https://cdn.hashnode.com/uploads/covers/6a06127cbaf09db7a628dd57/cc5d7227-2db3-4252-850c-a14dddd40a4e.png align="center")

  
The high level working of our model is shown in the below diagram.

![](https://cdn.hashnode.com/uploads/covers/6a06127cbaf09db7a628dd57/fb49469d-b7f3-4f9b-a5c5-0464a812e93e.png align="center")

We use the neural network to converts the input layer that might have 10000 vector encodings to 300 in the hidden layer and the output softmax layer gives a probability of each word as output.  
In the above example in the text document that we have had total of 10000 words.  
Now the process is repeated on the training set, weights are adjusted so that softmax gives the max probability to our specified word.

After the training is done. The hidden layer output is what we are interested in. Its the Embeddings.  
  
When our dataset is large enough for training. Then we get embeddings with same meaning words with highest cosine similarity.  
For implementation purpose, gensim is a python library where we can import word2vec.

```plaintext
from gensim.models import Word2Vec
```

  
  
The above word2vec is a static embedding model. By static, for all sentences a particular word would have the same vector embeddings.  

There are dynamic embedding models as well like BERT. BERT has a 12 layered architecture with a transformer model. These model generates different embedding vectors based on context.  
For Eg: for the word ball:  
I like to play with a ball \[based on sport\]  
I like to watch ball dance \[based on dance format\]  
  
Word2Vec will produce same vector embeddings for above. But Bert will produce different as they mean different.  
  
In short:  
Word2Vec turns sparse one‑hot vectors into compact, meaningful word vectors by training a simple two‑layer network to exploit distributional patterns in text. In practice you train either CBOW (predict a word from its surrounding context — faster, smooths over noise, works well for frequent words) or Skip‑Gram (predict surrounding words from a target — slower but better at capturing rare-word semantics). Both approaches scale to large corpora using approximations like negative sampling or hierarchical softmax, producing static embeddings whose cosine similarity reflects semantic relatedness.

Key practical points:

*   One‑hot → dense embeddings: learning projects words into a lower‑dimensional space where similar words cluster.
    
*   Hyperparameters matter: embedding dimension, context window, negative samples, and subsampling influence quality.
    
*   Limitations: embeddings are static (no context sensitivity) and suffer from **Out‑Of‑Vocabulary** words — use domain training or subword models (FastText) when needed.
    
*   Use cases: semantic search, clustering, feature inputs for downstream models, visualization and exploratory analysis.
    

Despite newer contextual models, Word2Vec remains a lightweight, interpretable, and effective tool for many NLP tasks — an excellent first step for turning raw text into numerical signals your models can learn from.