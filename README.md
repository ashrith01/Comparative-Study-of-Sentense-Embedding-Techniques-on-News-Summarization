# Comparative Study of Sentence Embedding Techniques on News Summarization

Which sentence encoder should you put underneath a graph-based extractive summariser? This study
holds the summarisation algorithm fixed — **LexRank** — and swaps out the sentence representation
five ways, then scores every summary against the human-written reference with ROUGE. Across 2,225
BBC News articles, **LaBSE** comes out on top.

![Python](https://img.shields.io/badge/Python-3776AB?logo=python&logoColor=white)
![sentence-transformers](https://img.shields.io/badge/sentence--transformers-FFB000?logo=huggingface&logoColor=black)
![TensorFlow Hub](https://img.shields.io/badge/TF%20Hub-FF6F00?logo=tensorflow&logoColor=white)
![NLTK](https://img.shields.io/badge/NLTK-154F5B)

## The question

Extractive summarisation picks sentences out of the source document rather than writing new ones.
LexRank does the picking: build a graph whose nodes are sentences and whose edge weights are
cosine similarities between sentence vectors, then rank the nodes by degree centrality and take
the top ones.

Everything about that procedure depends on the sentence vectors. So the vectors are the variable.

## Models compared

| Model | Source |
| --- | --- |
| **Universal Sentence Encoder (USE)** | `tfhub.dev/google/universal-sentence-encoder-large/5` |
| **Sentence-BERT** | `bert-base-nli-mean-tokens` |
| **MPNet** | `all-mpnet-base-v2` |
| **MiniLM** | `all-MiniLM-L6-v2` |
| **LaBSE** | `sentence-transformers/LaBSE` |

## Pipeline

```
BBC article ──▶ clean + sentence-split (NLTK)
            ──▶ encode each sentence  ◀── one of the five encoders
            ──▶ cosine similarity matrix
            ──▶ LexRank: degree centrality over the sentence graph
            ──▶ take top-ranked sentences as the summary
            ──▶ ROUGE-1 / ROUGE-2 / ROUGE-L against the human summary
```

## Dataset

The **BBC News Summary** dataset — 2,225 articles from 2004–2005 across five domains (business,
entertainment, politics, sport, tech), each paired with a human-written extractive summary.
Articles average 19 sentences; reference summaries average 2. Included here as
[`Dataset/BBC News Summary.zip`](Dataset/BBC%20News%20Summary.zip).

## Results

Best average F1 across all 2,225 articles, achieved by **LaBSE**:

| Metric | LaBSE F1 |
| --- | --- |
| ROUGE-1 | **0.5558** |
| ROUGE-2 | **0.4553** |
| ROUGE-L | **0.5501** |

LaBSE and USE were the two strongest encoders on every metric, with LaBSE ahead. Broken down by
domain, both did their best work on sports and business articles — domains with a fairly
formulaic structure, where the sentences that matter tend to be the ones most similar to
everything else in the piece.

That is worth noting as a limitation of the method rather than of the encoders: LexRank rewards
centrality, so it does well exactly where the important content is also the most representative
content, and less well on articles whose key sentence is an outlier.

## Running it

Open [`Code.ipynb`](Code.ipynb) and run it top to bottom. It expects the dataset unzipped
alongside; the notebook was written for Colab, so adjust the Drive-mount path if you run locally.

```bash
pip install sentence-transformers tensorflow tensorflow-hub nltk sumeval pandas matplotlib
```

## Repository layout

```
Code.ipynb    # encoders, LexRank, ROUGE scoring, per-domain analysis
Dataset/      # BBC News Summary corpus
Report.pdf    # full write-up
Poster.pptx
```

## Team

Course project at **Amrita School of Engineering, Bengaluru** (Amrita Vishwa Vidyapeetham), guided
by Dr. Deepa Gupta.

- K. Vishnu Sainadh
- K. Satwik
- V. Ashrith

Method, per-domain breakdowns and the comparison against prior work are in
[`Report.pdf`](Report.pdf).
