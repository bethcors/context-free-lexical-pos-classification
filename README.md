# A Context-Free Framework for Evaluating Morphological Generalization in Lexical POS Classification

This repository contains the reproducibility materials for the study:

**A Context-Free Framework for Evaluating Morphological Generalization in Lexical POS Classification**

**Authors:** Lilibeth P. Coronel, Emmylou A. Emperador, and Gergie A. Ambato  
**Affiliation:** Mindanao State University at Naawan, Philippines

## Overview

This study investigates how much part-of-speech (POS) information can be inferred from isolated English word forms without sentence context.

The experimental framework evaluates a majority-class baseline and four morphology-sensitive approaches:

1. Majority-class baseline
2. Rule-based suffix baseline
3. Statistical suffix model
4. Character N-gram linear model
5. Character-level BiLSTM

Models are evaluated under coarse-grained and fine-grained lexical POS classification.

Morphological generalization is further examined using a suffix hold-out protocol in which selected morphological families are excluded from model training.

## Dataset

The experiments use the **370k English Words Corpus** distributed through Kaggle. The dataset contains 370,100 English word types with automatically assigned Penn Treebank-style POS labels.

The original dataset is **not redistributed in this repository**. Users should obtain the dataset from its original source:

**Kaggle dataset:**  
Ruchi Bhatia, *Part-of-Speech Tagging / 370k English Words Corpus*  
https://www.kaggle.com/datasets/ruchi798/part-of-speech-tagging

Before analysis, lexical forms are stripped of surrounding whitespace and converted to lowercase. The dataset is audited for missing values, duplicate lexical forms, duplicate word-label pairs, and nonalphabetic entries.

See `data/README.md` for data preparation instructions.

## Experimental Tasks

### Coarse-Grained Classification

Penn Treebank-style POS tags are mapped into five broad grammatical categories:

- NOUN
- VERB
- ADJ
- ADV
- FUNCTION

### Fine-Grained Classification

The fine-grained experiment retains Penn Treebank-style POS categories represented by at least 50 observations:

- NN
- NNS
- JJ
- RB
- VBG
- VBN
- VB
- VBD
- JJS
- IN

### Suffix Hold-Out Evaluation

Morphological transfer is evaluated by withholding complete suffix families from training.

The primary held-out suffixes are:

- `-tion`
- `-ness`
- `-ly`
- `-ing`

Each lexical item is assigned to its longest matching suffix family. For each experiment, all words belonging to the target suffix family are reserved as the test set.

For the rule-based model, the corresponding target suffix rule is removed during primary hold-out evaluation. A full-rule condition is retained in the reproducibility outputs only as a diagnostic reference and is excluded from the primary model comparison.

## Computational Models

### Majority-Class Baseline

Predicts the most frequent POS category in the corresponding training partition.

### Rule-Based Suffix Baseline

Uses manually specified suffix-to-POS mappings. When multiple suffixes match, the longest matching suffix is used. Words without a matching rule receive the majority class.

### Statistical Suffix Model

Learns empirical suffix-label frequencies from the training data using suffixes of one to five characters. Prediction uses the longest matching suffix observed during training.

### Character N-gram Linear Model

Represents words using hashed character n-grams of lengths 2–5 and performs classification using stochastic gradient descent with logistic loss.

The hashing space contains 262,144 dimensions (`2^18`). The observed hash-collision statistics are provided in:

`results/hash_collision_audit.json`

### Character-Level BiLSTM

Represents each word as a character sequence and uses a bidirectional LSTM to learn character-level lexical representations for POS classification.

## Data Partitioning

For the standard coarse- and fine-grained experiments, the data are divided using stratified sampling:

- Training: 70%
- Validation: 15%
- Test: 15%
- Random seed: 42

For each suffix hold-out experiment, the complete target suffix family is reserved for testing. The remaining observations are divided into 85% training and 15% validation data using stratified sampling and random seed 42.

## Evaluation

**Macro-F1** is the primary evaluation metric because of substantial class imbalance. **Accuracy** is reported as a complementary measure.

For standard coarse- and fine-grained experiments, 95% confidence intervals are estimated using 2,000 bootstrap repetitions.

Because the FUNCTION category contains only 27 observations in the coarse-grained test partition, FUNCTION recall is additionally reported with an exact Clopper-Pearson 95% confidence interval.

Adjacent model comparisons are evaluated using paired randomization tests with 2,000 repetitions:

- Rule-based suffix vs. Statistical suffix
- Statistical suffix vs. Char N-gram
- Char N-gram vs. BiLSTM

Raw p-values are adjusted using the Holm procedure.

For suffix hold-out experiments, Macro-F1 is calculated only over POS classes represented in the ground-truth test subset for each suffix family.

## Repository Structure

```text
.
├── README.md
├── LICENSE
├── CITATION.cff
├── requirements.txt
├── .gitignore
├── config/
│   └── experimental_configuration.json
├── notebooks/
│   └── lexical_pos_classification_experiments.ipynb
├── data/
│   └── README.md
└── results/
    ├── standard_results.csv
    ├── suffix_holdout_per_family.csv
    ├── suffix_holdout_classwise.csv
    ├── function_class_results.csv
    ├── hash_collision_audit.json
    ├── paired_randomization_tests.csv
    └── predictions/
        ├── coarse_test_predictions.csv
        └── fine_test_predictions.csv
