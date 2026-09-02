# Dataset

**BBC News Summary** — 2,225 BBC news articles from 2004–2005, each paired with a human-written
extractive summary. Included in this repository as
[`Dataset/BBC News Summary.zip`](Dataset/BBC%20News%20Summary.zip).

## Structure

```
BBC News Summary/
├── News Articles/
│   ├── business/       *.txt
│   ├── entertainment/  *.txt
│   ├── politics/       *.txt
│   ├── sport/          *.txt
│   └── tech/           *.txt
└── Summaries/
    └── (same five folders, one summary per article)
```

The article's headline is the first line of each article file.

## Statistics

| | Value |
| --- | --- |
| Articles | 2,225 |
| Domains | 5 (business, entertainment, politics, sport, tech) |
| Mean sentences per article | 19 |
| Mean sentences per reference summary | 2 |

The roughly 10:1 compression ratio is what makes this a usable benchmark for extractive
summarisation: the summariser has to select about two sentences out of nineteen, so the choice
actually discriminates between methods.

## Usage

Unzip alongside the notebook and point the dataset path in
`notebooks/summarization_comparison.ipynb` at the extracted folder.

## Source

Originally derived from the BBC news dataset released with Greene & Cunningham (2006), *Practical
Solutions to the Problem of Diagonal Dominance in Kernel Document Clustering*, ICML.
