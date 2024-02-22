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

Each language has its own istance of the model, which is trained on the respective bag-of-words documents.

The models encodes the tokens in vectors of size 100 that are then processed with a Robust PCA which keeps only the 5 most informative components of the embeddings.

|Languages|Number of tokens|Embeddings dimension|
|:--|--:|--:|
|alt.atheism|305|5|
|soc.religion.christian|546|5|
|talk.politics.guns|460|5|

## Topological Data Analysis (TDA)

### Persistent Homology with Vietoris-Rips Complexes

In this section we analyse the persistent homology of each semantic space by fitting a Vietoris-Rips filtration on the embeddings of each language.



### Mapper Algorithm and Signal Analysis

#### Toxicity Signal

![toxicity signal](./img/words_toxicity_score.svg)

> The boxplot of the word/entity toxicity score for each language.

In this study, our objective is to evaluate whether the graph topologies of different languages capture some of the structural aspects of the toxicity signal.

Each document is scored from 0 to 100 by the Google [Perspective API](https://perspectiveapi.com/), which calculates how much a document is toxic. Then the score is propragated to every single words $w_i$ as:

$$
\begin{gather}
S(w_i) = \frac{1}{|D_i|}\sum_{d\in D_i} S(d_j)\\
\text{where }D_i\text{ is the set of documents in which }w_i\text{ appears.}
\end{gather}
$$

<p float="left">
  <img src="./img/alt.atheism_frequency_spectrum.svg" width="300" />
  <img src="./img/soc.religion.christian_frequency_spectrum.svg" width="300" /> 
  <img src="./img/talk.politics.guns_frequency_spectrum.svg" width="300" />
</p>


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