# NLP Systems Portfolio

[English](README.md) | [한국어](README.ko.md)

Two end-to-end natural language processing projects covering representation learning, semantic similarity, recurrent networks, transformer fine-tuning, class imbalance, and ensemble inference. The repository is organized for portfolio review: each project has a focused overview, readable notebooks, data, final predictions, and the original task reference.

## Projects

| Project | Problem | Approaches | Best reported result |
| --- | --- | --- | ---: |
| [Word Similarity](projects/word-similarity/) | Score the semantic similarity of word and phrase pairs | TF-IDF + character n-grams; FastText + phrase detection | 69.9% evaluation success rate |
| [Multi-label Film Attribute Classification](projects/film-attribute-classification/) | Assign any combination of eight attributes to a film synopsis | BiLSTM + attention; RoBERTa; DeBERTa-v3 ensemble | 0.6253 weighted F1 |

## Selected engineering work

- Designed two complementary out-of-vocabulary strategies: character n-grams for sparse vectors and FastText subword embeddings for dense vectors.
- Combined word-level and character-level representations while caching repeated vector construction to reduce inference overhead.
- Used Asymmetric Loss and class-specific threshold search to address imbalance in an eight-label classification problem.
- Compared a recurrent baseline with two transformer pipelines instead of assuming the largest model would perform best.
- Applied automatic mixed precision, layer-wise learning-rate decay, masked mean pooling, and a three-seed soft-voting ensemble.

## Repository layout

```text
.
├── README.md
├── README.ko.md
└── projects/
    ├── word-similarity/
    │   ├── data/
    │   ├── notebooks/
    │   ├── references/
    │   ├── results/
    │   ├── README.md
    │   └── README.ko.md
    └── film-attribute-classification/
        ├── data/
        ├── notebooks/
        ├── references/
        ├── results/
        ├── README.md
        └── README.ko.md
```

## Reproducing the work

1. Open the README for the project you want to explore.
2. Start Jupyter from that project directory so the relative `data/` and `results/` paths resolve correctly.
3. Install the dependencies listed in the notebook and run the cells in order.

The Word Similarity project additionally requires a local WikiText-103 text file. The film-classification notebooks download model or embedding assets on first use, and the transformer experiments are intended for a CUDA-capable GPU.

Notebook outputs and execution counters are intentionally cleared for fast review. Reported metrics are preserved in the documentation, while final prediction files remain under each project's `results/` directory.

## Repository hygiene

Daily notes, private work logs, scratch experiments, checkpoints, downloaded model weights, experiment trackers, and generated validation predictions are excluded through `.gitignore`. The local submission archives are preserved under the ignored `.local/` directory and will not be uploaded to GitHub.

This repository contains academic project work. The task briefs are retained only as project references; no license is granted for third-party datasets or course materials.
