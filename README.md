# context-free-lexical-pos-classification
Reproducibility materials for context-free lexical POS classification and suffix hold-out evaluation of morphological generalization.
# A Context-Free Framework for Evaluating Morphological Generalization in Lexical POS Classification

This repository contains the code, experimental configurations, data-split specifications, evaluation procedures, and reproducibility materials associated with the study:

**A Context-Free Framework for Evaluating Morphological Generalization in Lexical POS Classification**

**Authors:**  
Lilibeth P. Coronel  
Emmylou A. Emperador  
Gergie A. Ambato  

Mindanao State University at Naawan, Philippines

## Overview

This study investigates how much grammatical information can be inferred from isolated lexical forms without sentence context. It evaluates context-free lexical part-of-speech (POS) classification using a majority-class baseline and four morphology-sensitive computational approaches:

1. Rule-Based Suffix Baseline
2. Statistical Suffix Model
3. Character N-gram Model
4. Character-Level BiLSTM

The models are evaluated under coarse-grained and fine-grained POS classification settings.

The study also introduces a suffix hold-out evaluation protocol to examine whether model performance transfers to morphological families excluded from training. The primary held-out suffix families are:

- `-tion`
- `-ness`
- `-ly`
- `-ing`

## Dataset

The experiments use the **370k English Words Corpus** distributed through Kaggle.

The source dataset contains 370,100 English word types with automatically assigned POS labels. The study uses Penn Treebank-style POS categories.

The original dataset is **not redistributed in this repository**. Users should obtain the dataset from its original source.

Dataset source:

**370k English Words Corpus / Part-of-Speech Tagging Dataset**  
Kaggle, distributed by Ruchi Bhatia.

Additional information about obtaining and preparing the dataset is provided in:

`data/README.md`

## Experimental Tasks

### 1. Coarse-Grained Lexical POS Classification

Penn Treebank-style POS tags are consolidated into five grammatical categories:

- NOUN
- VERB
- ADJ
- ADV
- FUNCTION

### 2. Fine-Grained Lexical POS Classification

The original Penn Treebank-style POS categories are retained for categories represented by at least 50 observations.

### 3. Suffix Hold-Out Evaluation

For each target suffix family, all lexical forms belonging to that family are excluded from model training and reserved for testing.

The remaining non-held-out observations are divided into training and validation sets.

For the rule-based model, the rule corresponding to the target suffix is removed during evaluation. This produces the **Rule-Masked** baseline used in the primary comparison.

Macro-F1 for each held-out family is calculated only over POS categories represented in that family's ground-truth test set.

## Computational Models

### Majority-Class Baseline

Predicts the most frequent POS category in the corresponding training partition.

### Rule-Based Suffix Baseline

Uses manually specified suffix-to-POS mappings. When multiple suffixes match a lexical form, the longest matching suffix is used.

### Statistical Suffix Model

Learns empirical suffix-to-POS associations from the training data using suffixes of lengths 1–5 characters.

### Character N-gram Model

Represents lexical forms using hashed character n-grams of lengths 2–5 and performs classification using stochastic gradient descent with logistic loss.

### Character-Level BiLSTM

Represents each lexical form as a character sequence and learns a bidirectional recurrent representation for POS classification.

## Data Partitioning

For the standard coarse- and fine-grained experiments, the dataset is divided using stratified sampling:

- Training: 70%
- Validation: 15%
- Test: 15%
- Random seed: 42

For each suffix hold-out experiment:

- All members of the target suffix family are reserved for testing.
- The remaining observations are divided into 85% training and 15% validation data.
- Random seed: 42

All normalized lexical forms in the source dataset are unique, preventing exact lexical-form overlap between the standard training, validation, and test partitions.

## Evaluation

The primary evaluation metric is **Macro-F1**. Accuracy is reported as a complementary metric for the standard classification experiments.

Statistical analysis includes:

- 2,000 bootstrap repetitions for 95% confidence intervals
- Exact Clopper-Pearson confidence intervals for FUNCTION-class recall
- Paired randomization tests with 2,000 repetitions
- Holm correction for multiple pairwise comparisons

For suffix hold-out evaluation, zero-support POS categories are excluded from the family-specific Macro-F1 calculation.

## Repository Structure

```text
context-free-lexical-pos-classification/
│
├── README.md
├── LICENSE
├── .gitignore
├── requirements.txt
├── CITATION.cff
│
├── notebooks/
│   └── lexical_pos_classification_experiments.ipynb
│
├── config/
│   ├── experimental_configuration.json
│
├── data/
│   └── README.md
│
├── splits/
│   ├── README.md
│   └── standard_split_indices.csv
│
├── results/
│   ├── coarse_test_predictions.csv
│   ├── fine_test_predictions.csv
│   ├── standard_results.csv
│   ├── suffix_holdout_classwise.csv
│   ├── suffix_holdout_per_family.csv
│   ├── paired_tests_coarse.csv
│   ├── paired_tests_fine.csv
│   └── function_class_results.csv
│   └── hash_collision_audit.csv
│
└── figures/
    ├── convergence/
    └── suffix_holdout_macro_f1.png
