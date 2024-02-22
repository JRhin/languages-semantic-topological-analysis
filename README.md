# Topological Analysis of Languages Semantic Spaces

The idea behind this project is to analyse the differences between the semantic spaces of languages, where the semantic space is captured by the latent space of an encoding model.

To analyse the differences between spaces we perform an in depth topological analysis of the space structure, in particular:

- Persistence Analysis of simplicial complex built from the words embeddings.
- Graph Analysis of the graph resulting from the Mapper Algorithm. 

## Dataset

For our analysis we're going to use the `fetch_20newsgroups` dataset available on scikit-learn ([link](https://scikit-learn.org/stable/datasets/real_world.html#newsgroups-dataset) to the documentation). In particular we focus on 3 of the 20 topics:

- alt.atheism
- soc.religion.christian
- talk.politics.guns

Each specific topic defines a "language" to which a user can adhere.

## Cleaning and Preprocessing

For each document in the dataset we have removed the headers, the footer and the quotes, keeping only the main text which is then parsed using a personalized [SpaCy](https://spacy.io/) Tokenizer, that:

- Keeps only the lemma of a word.
- Keeps only nouns, adjectives, verbs and pronouns.
- Merges entities in one token.
- Changes negated verbs in "not_[lemma of verb]".

Each document now is seen as a special bag-of-words.

|Language|Number of tokens|
|:--|--:|
|alt.atheism|5747|
|soc.religion.christian|7599|
|talk.politics.guns|6676|

Because the topological analysis we're going to perform are computationally expensive we would like to reduce the number of tokens keeping only the most important ones.

![ecdf plot](./img/ecdf.svg)

> The complementary ECDF of the word document frequency (left) and of the word frequency in the language (right).

From the complementary ECDF plots we can notice that the majority of the tokens appear few times both in the entire corpus and both in the documents, while there is only a small portion of them that have both high frequencies. This consideration leds to the following assumption:

**Assumption**: tokens that have a large frequency are characteristic of a language and retain the major informations about its structure.

The assumption considers stop words also characteristical terms of a language (because they usually have high frequency), but thanks to the previous tokenization step stop words are automatically removed.

So the analysis consider only tokens that appear at least 10 times in the overall corpus and in 15 different documents.

|Languages|Number of tokens|
|:--|--:|
|alt.atheism|305|
|soc.religion.christian|546|
|talk.politics.guns|460|

Each languages has its own istance of the model, which is trained on the respective bag-of-words documents.

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