# Data

## Dataset Source

The experiments use the **370k English Words Corpus** distributed
through Kaggle:

**Ruchi Bhatia --- Part-of-Speech Tagging / 370k English Words Corpus**

https://www.kaggle.com/datasets/ruchi798/part-of-speech-tagging

The dataset contains English lexical forms with automatically assigned
Penn Treebank-style part-of-speech (POS) labels. The dataset used in
this study contains **370,100 word types**.

## Dataset Availability

The original dataset is **not redistributed in this repository**. To
reproduce the experiments, obtain the dataset directly from the original
Kaggle source and comply with the terms of the source dataset.

## Expected File and Location

Prepare the input dataset as:

``` text
data/words_pos.csv
```

The file should contain the lexical word forms and their corresponding
POS tags in the format expected by:

``` text
notebooks/lexical_pos_classification_experiments.ipynb
```

## Preprocessing

The reproducibility notebook performs the preprocessing and dataset
checks used in the study:

-   strips surrounding whitespace from lexical forms;
-   converts words to lowercase;
-   checks for missing values;
-   checks for duplicate lexical forms;
-   checks for duplicate word-label pairs; and
-   checks for nonalphabetic lexical forms.

After these checks, the dataset used in the reported experiments
contained **370,100 normalized word forms**, with each word associated
with one supplied POS label.

The single supplied POS label should not be interpreted as evidence that
English lexical forms are inherently grammatically unambiguous. The
experiments evaluate predictions relative to the POS labels supplied by
the source dataset.
