# Topological Analysis of Languages Semantic Spaces

The idea behind this project is to analyse the differences between the semantic spaces of languages, where the semantic space is captured by the latent space of an encoding model.
To analyse the differences between spaces we perform an in depth topological analysis of the space structure, in particular:

- Persistence Analysis of simplicial complex built from the words embeddings.
- Graph Analysis of the graph resulting from the Mapper Algorithm. 

## Dataset

For our analysis we're going to use the `fetch_20newsgroups` dataset available on scikit-learn. In particular we focus on 3 of the 20 topics:

- alt.atheism
- soc.religion.christian
- talk.politics.guns

Each specific topic defines a "language" to which a user can adhere.

## Cleaning and Preprocessing

For each document in the dataset we have removed the headers, the footer and the quotes, keeping only the main text that we're going to parse using a personalized [SpaCy](https://spacy.io/) Tokenizer:

- Keep only the lemma of a word.
- Keeps only nouns, adjectives, verbs and pronouns.
- Entities are merged in one token.
- Negated verbs are rewritten as "not_<lemma of verb>".

So each document is seen as a special bag-of-words which is going to used to train a Word2Vec model (the Encoding Model).

![ecdf plot](./img/ecdf.svg)

> The complementary ECDF of the word document frequency (left) and of the word frequency in the language (right). 

Each languages has its own istance of the model.

## Directory Structure

```bash
./root
  |_ img/
  |_ notebook.ipynb
  |_ README.md
```

## Team

- [Mario Edoardo Pandolfo](https://github.com/JRhin)

## Used Technologies

![Python](https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54) ![Plotly](https://img.shields.io/badge/Plotly-%233F4F75.svg?style=for-the-badge&logo=plotly&logoColor=white) ![scikit-learn](https://img.shields.io/badge/scikit--learn-%23F7931E.svg?style=for-the-badge&logo=scikit-learn&logoColor=white) ![Jupyter Notebook](https://img.shields.io/badge/jupyter-%23FA0F00.svg?style=for-the-badge&logo=jupyter&logoColor=white)