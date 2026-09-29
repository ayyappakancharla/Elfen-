# Evaluating ELFEN for Linguistic Feature Extraction

## Project Overview

This project evaluates **ELFEN**, a Python package for linguistic
feature extraction, using a dataset of **1,000 English sentences**.

The evaluation focuses on:

-   linguistic feature coverage
-   experimental DataFrame output
-   external resource dependencies
-   missing-value behaviour
-   reproducibility of the extraction pipeline
-   implementation of Flesch Reading Ease

## Poster

**Title:**\
**Evaluating ELFEN for Linguistic Feature Extraction --- Feature
Coverage, External Resource Dependencies, and Reproducibility**

The poster presents the experimental results from running ELFEN on the
1,000-sentence English dataset.

## Research Questions

1.  Which types of linguistic features does ELFEN successfully extract
    from a standard English text dataset?
2.  Which feature groups depend on external linguistic resources?
3.  What failure modes occur when required resources are missing?
4.  Does the absence of specific resources prevent complete execution of
    `extract_features()`?
5.  Do ELFEN's implemented readability formulas reproduce the
    corresponding standard formulas?

## Dataset

-   **Language:** English
-   **Number of sentences:** 1,000
-   **Input columns:** `id`, `text`
-   **Dataset file:** `english_sentance_elfen_1000_named.csv`

The dataset contains 1,000 unique English sentences and was used as the
common input for the feature-extraction experiments.

## Experimental Environment

-   **ELFEN:** 1.3.1
-   **Python:** 3.11
-   **Backbone:** spaCy
-   **English model:** `en_core_web_sm`
-   **WordNet package:** `wn` 1.1.0
-   **WordNet resource:** EWN 2020

Additional NRC resources were installed for emotion/sentiment
extraction.

## Feature Coverage

ELFEN documents **1,061 linguistic features across 11 areas**:

  Linguistic area            Documented features
  ------------------------ ---------------------
  Surface-Level                               11
  Lexical Richness                            26
  Readability                                 11
  Named Entities                              19
  Information Theory                           2
  Emotion / Sentiment                         36
  POS                                         20
  Psycholinguistics                           78
  Semantics                                   17
  Morphology                                 798
  Syntactic Dependencies                      43
  **Total**                            **1,061**

Morphology is the largest documented feature group, with **798
features**.

## Experimental DataFrame Output

The final experimental DataFrame contained:

-   **3 base columns:** `id`, `text`, `nlp`
-   **1,103 non-base output/helper columns**
-   **1,106 total columns**

The 1,106-column result should **not** be interpreted as 1,106
documented linguistic features. It is the final DataFrame column count.

## External Resource Dependencies

Several feature groups depend on external linguistic resources.

### Semantics

Semantic extraction initially failed because the required English
WordNet resource was unavailable:

`WordNet not found for 'en'`

After installing **EWN 2020**, semantic extraction could proceed.

### Emotion / Sentiment

Emotion-related extraction required NRC resources. After the required
NRC resources were installed, emotion extraction completed.

### Psycholinguistics

Psycholinguistic features rely on linguistic norm resources. Some words
in the dataset were not covered by the available norms, resulting in
partial missingness during isolated psycholinguistic extraction.

## Missingness Results

  Extraction stage                         Missingness
  -------------------------------------- -------------
  Isolated emotion extraction               **38.63%**
  Isolated psycholinguistic extraction       **0.76%**
  Final full pipeline                           **0%**

The isolated emotion result contained **26,652 missing cells out of
69,000**, while the isolated psycholinguistic result contained **976
missing cells out of 128,000**.

After the required resources were installed, the final full extraction
completed with **0 missing cells across 1,106,000 DataFrame cells**.

The isolated feature-group results and the final full-pipeline result
represent different extraction stages and should therefore not be
treated as contradictory measurements.

## Flesch Reading Ease: Standard vs. ELFEN 1.3.1

A notable unexpected result was observed for **Flesch Reading Ease**.

### Standard Flesch Reading Ease formula

The standard calculation used for comparison is:

**FRE = 206.835 − 1.015 × (words / sentences) − 84.6 × (syllables /
words)**

For the first sentence in the dataset:

-   **Words/tokens:** 14
-   **Sentences:** 1
-   **Syllables:** 13

The standard calculation is:

**FRE = 206.835 − 1.015 × (14 / 1) − 84.6 × (13 / 14)**

**FRE ≈ 114.07**

### Installed ELFEN 1.3.1 implementation

The installed ELFEN 1.3.1 implementation uses:

**FRE = 206.835 − 1.015 × (words / sentences) − 84.6 × (syllables /
sentences)**

Using the same values:

**FRE = 206.835 − 1.015 × (14 / 1) − 84.6 × (13 / 1)**

**FRE = −907.175**

The negative value observed in the dataset can therefore be reproduced
directly from the installed ELFEN 1.3.1 implementation.

### Comparison

  Calculation                    Flesch Reading Ease
  ---------------------------- ---------------------
  Standard formula                        **114.07**
  ELFEN 1.3.1 implementation            **−907.175**

This comparison identifies an **implementation discrepancy in the tested
ELFEN 1.3.1 version** between the standard Flesch calculation and the
installed implementation.

### Dataset-level observation

Across the 1,000-sentence dataset, the ELFEN-generated Flesch Reading
Ease values had:

-   **Mean:** −813.20
-   **Median:** −731.88
-   **Standard deviation:** 520.37
-   **Minimum:** −3557.18
-   **Maximum:** −51.02

This calculation comparison is an implementation audit of the tested
ELFEN version and configuration. It should not be interpreted as a
general claim about all ELFEN versions or configurations.

## Other Readability Statistics

  Measure                     Mean
  ---------------------- ---------
  Flesch Reading Ease      −813.20
  Flesch-Kincaid Grade      129.37
  SMOG                        5.49
  Gunning Fog                 6.14
  Coleman-Liau Index          2.45
  LIX                        29.54
  RIX                         2.04

## Key Findings

1.  ELFEN documents **1,061 features across 11 linguistic areas**.
2.  **Morphology accounts for 798 documented features**.
3.  The experiment produced a final DataFrame with **1,106 columns**.
4.  **WordNet and NRC resources were required** for complete semantic
    and emotion-related extraction.
5.  Isolated feature groups showed different levels of missingness
    because of incomplete external resource coverage.
6.  After installing the required resources, the full pipeline completed
    with **0% missing cells**.
7.  The tested ELFEN 1.3.1 implementation produced unusually negative
    Flesch Reading Ease scores that could be reproduced from its
    implementation.

## Limitations / Interpretation

-   **External resources affect reproducibility:** WordNet and NRC
    resources were required for complete extraction.
-   **Output is not the feature inventory:** ELFEN documents 1,061
    features, while the final DataFrame contained 1,106 columns.
-   **Dataset scope:** Results are based on 1,000 English sentences and
    may vary across datasets, domains, or languages.
-   **Readability anomaly:** The negative Flesch scores were
    reproducible from the tested ELFEN 1.3.1 implementation.

## Conclusion

ELFEN enabled broad linguistic feature extraction for the 1,000-sentence
dataset after the required resources were installed. The evaluation
revealed resource-dependent extraction, incomplete coverage in some
isolated feature groups, and a reproducible discrepancy in the Flesch
Reading Ease implementation. These results highlight the importance of
validating external resources and feature implementations when using
automated linguistic extraction tools.

## Reproducibility

To reproduce the main experiment:

1.  Install Python 3.11.
2.  Install ELFEN 1.3.1 and its required dependencies.
3.  Install the English spaCy model `en_core_web_sm`.
4.  Install the `wn` package and download EWN 2020.
5.  Install the required NRC emotion/sentiment resources.
6.  Load the 1,000-sentence CSV dataset.
7.  Configure ELFEN with the English spaCy backbone.
8.  Run the feature extraction pipeline.
9.  Inspect the resulting DataFrame for feature count, missingness, and
    readability values.

Package versions and external resource versions should be recorded
because resource availability and coverage can affect the output.

## Academic Note

The results reported here describe the specific ELFEN version,
resources, configuration, and dataset used in this experiment. Numerical
results should therefore be interpreted as experimental observations
from this setup rather than as universal properties of ELFEN.
