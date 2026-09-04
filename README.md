# COMP34711 Natural Language Processing Coursework

Coursework submissions for COMP34711 (2025/26). The repository contains two independent pieces of work: word-similarity prediction from a large text corpus, and multi-label classification of film plots.

The notebooks are the source of truth for the experiments. Submission CSVs are included alongside them in the relevant `SUBMIT` / `Submit` folders.

## Contents

| Folder | Topic | Main work |
| --- | --- | --- |
| [`Corsework 1`](Corsework%201) | Word similarity | Two approaches to estimating the similarity of word pairs from WikiText-103 |
| [`Corsework 2`](Corsework%202) | Film-plot classification | Three multi-label classifiers for eight film-related labels |

> The directory name `Corsework` is retained to match the submitted coursework structure.

## Coursework 1 - Word similarity

Given pairs of words, the task is to produce a similarity score. Both notebooks use WikiText-103 as their training corpus and write predictions for the provided 160-row test set.

### Task 1: TF-IDF with character n-grams

[`10879360_CW1_task1.ipynb`](Corsework%201/SUBMIT/10879360_CW1_task1.ipynb) builds a sparse representation from two TF-IDF vectorisers:

- word unigrams, limited to 50,000 features and filtered at document frequency 5;
- character-boundary n-grams of length 3-5, limited to 30,000 features and filtered at document frequency 10.

The word and character vectors are combined before cosine similarity is calculated. The character component provides a fallback for words missing from the word-level vocabulary, while the preprocessing also handles multi-word terms.

Output: [`10879360_CW1_task1_results.csv`](Corsework%201/SUBMIT/10879360_CW1_task1_results.csv)

### Task 2: FastText with phrase detection

[`10879360_CW1_task2.ipynb`](Corsework%201/SUBMIT/10879360_CW1_task2.ipynb) first learns bigram phrases (`min_count=5`, `threshold=10`) and then trains a skip-gram FastText model. The model uses 150-dimensional vectors, a context window of 5, a minimum token count of 5, five training epochs, and all available CPU workers.

FastText's subword representation is used for words not seen as complete tokens during training.

Output: [`10879360_CW1_task2_results.csv`](Corsework%201/SUBMIT/10879360_CW1_task2_results.csv)

The provided files [`CW1_examples.csv`](Corsework%201/CW1_examples.csv), [`CW1_examples_gold.csv`](Corsework%201/CW1_examples_gold.csv), and [`CW1_testdata.csv`](Corsework%201/CW1_testdata.csv) contain 100 example pairs and 160 test pairs respectively. The WikiText-103 corpus itself is not stored in this repository.

## Coursework 2 - Multi-label film-plot classification

The input is a film title and plot synopsis. Each example may belong to any combination of these labels, in this fixed order:

`comedy`, `cult`, `flashback`, `historical`, `revenge`, `romantic`, `scifi`, `violence`.

The dataset files are [`CW2_training_dataset.csv`](Corsework%202/CW2_training_dataset.csv), [`CW2_validation_dataset.csv`](Corsework%202/CW2_validation_dataset.csv), and [`CW2_test_dataset.csv`](Corsework%202/CW2_test_dataset.csv). The notebooks report weighted F1 on the 1,031-example validation set and use class-specific decision thresholds selected on that set.

| Task | Notebook | Model | Best reported validation weighted F1 |
| --- | --- | --- | ---: |
| 1 | [`task1`](Corsework%202/Submit/10879360_task1_code.ipynb) | BiLSTM with attention and GloVe embeddings | 0.5845 |
| 2 | [`task2`](Corsework%202/Submit/10879360_task2_code.ipynb) | Fine-tuned `roberta-base` | 0.6253 |
| 3 | [`task3`](Corsework%202/Submit/10879360_task3_code.ipynb) | `microsoft/deberta-v3-base` with mean pooling and a three-seed ensemble | 0.6142 |

### Common choices

All three CW2 models treat the problem as multi-label classification and use Asymmetric Loss to reduce the influence of easy negative examples. Rather than applying one universal probability cutoff, each notebook searches for a threshold per label on the validation predictions.

- **Task 1** uses 300-dimensional GloVe embeddings with a bidirectional LSTM and attention layer. Sequences are capped at 1,400 tokens; training runs for 10 epochs with batch size 64.
- **Task 2** fine-tunes `roberta-base` for 10 epochs, with a maximum sequence length of 512 and batch size 32. It uses automatic mixed precision and layer-wise learning-rate decay.
- **Task 3** uses a custom classifier over DeBERTa-v3 hidden states, mean-pooling the valid token representations. Three runs (seeds 16, 42, and 378) are combined for the final predictions. Training uses a 512-token limit, batch size 12, and 10 epochs per seed.

The Task 3 notebook also records the best individual seed score (0.6223); the table reports the validation score of the final submitted ensemble predictions.

### Submission files

Each CSV contains the 1,053 test-set IDs followed by eight binary predictions in the label order above.

- [`10879360_task1_results.csv`](Corsework%202/Submit/10879360_task1_results.csv)
- [`10879360_task2_results.csv`](Corsework%202/Submit/10879360_task2_results.csv)
- [`10879360_task3_results.csv`](Corsework%202/Submit/10879360_task3_results.csv)

## Running the notebooks

The work was prepared as Jupyter/Colab notebooks. Open the notebook for the task you want to inspect and update the path constants near the top if your data is elsewhere.

CW1 requires Python with `pandas`, `numpy`, `scikit-learn`, `scipy`, `nltk`, and (for Task 2) `gensim`, as well as a local WikiText-103 text file. CW2 installs the required packages in its notebooks and downloads the model or embedding assets when first run. A GPU is strongly recommended for the RoBERTa and DeBERTa experiments.

## Notes on reproducibility

The repository keeps the submitted notebooks and prediction files, but not downloaded assets such as WikiText-103, GloVe vectors, or fine-tuned checkpoint files. Re-running a notebook therefore requires access to those external assets. Results can also vary with hardware, library versions, and GPU execution; random seeds are fixed in the CW2 notebooks where applicable.
