# NLP Systems Portfolio

[English](README.md) | [한국어](README.ko.md)

Two end-to-end natural language processing projects covering representation learning, semantic similarity, recurrent networks, transformer fine-tuning, class imbalance, and ensemble inference. The repository is organized for portfolio review: each project has a focused overview, readable notebooks, reproducible environment requirements, data, and final predictions.

## Projects

| Project | Problem | Approaches | Best reported result |
| --- | --- | --- | ---: |
| [Word Similarity](projects/word-similarity/) | Score the semantic similarity of word and phrase pairs | TF-IDF + character n-grams; FastText + phrase detection | 69.9% evaluation success rate |
| [Multi-label Film Attribute Classification](projects/film-attribute-classification/) | Assign any combination of eight attributes to a film synopsis | BiLSTM + attention; RoBERTa; DeBERTa-v3 ensemble | 0.6253 weighted F1 |

## Results at a glance

| Word similarity | Film attribute classification |
| --- | --- |
| ![Word similarity model comparison](projects/word-similarity/assets/model-comparison.svg) | ![Film attribute classification model comparison](projects/film-attribute-classification/assets/model-comparison.svg) |

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
    │   ├── assets/
    │   ├── data/
    │   ├── notebooks/
    │   ├── results/
    │   ├── README.md
    │   ├── README.ko.md
    │   └── requirements.txt
    └── film-attribute-classification/
        ├── assets/
        ├── data/
        ├── notebooks/
        ├── results/
        ├── README.md
        ├── README.ko.md
        └── requirements.txt
```

## Reproducing the work

Each project has its own dependency file. From the selected project directory, create and activate an isolated Python 3.10 or 3.11 environment, then launch Jupyter:

```bash
python -m venv .venv

# Activate one environment:
# Windows PowerShell: .venv\Scripts\Activate.ps1
# macOS/Linux:        source .venv/bin/activate

python -m pip install --upgrade pip
python -m pip install -r requirements.txt
python -m jupyter lab
```

Project-specific data placement, NLTK resources, model downloads, and GPU notes are documented in each project README.

The Word Similarity project additionally requires a local WikiText-103 text file. The film-classification notebooks download model or embedding assets on first use, and the transformer experiments are intended for a CUDA-capable GPU.

Notebook outputs and execution counters are intentionally cleared for fast review. Reported metrics are preserved in the documentation, while final prediction files remain under each project's `results/` directory.

## Repository hygiene

Daily notes, private work logs, scratch experiments, checkpoints, downloaded model weights, experiment trackers, and generated validation predictions are excluded through `.gitignore`. The local submission archives are preserved under the ignored `.local/` directory and will not be uploaded to GitHub.

These systems were developed as university projects and curated here as engineering case studies. The academic origin provides the problem context; the portfolio documentation focuses on implementation decisions, measured results, trade-offs, and reproducibility.
