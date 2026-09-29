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
-   the implementation of Flesch Reading Ease

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

## Readability Result

A notable unexpected result was observed for **Flesch Reading Ease**.

For the 1,000-sentence dataset:

-   **Mean:** −813.20
-   **Median:** −731.88
-   **Standard deviation:** 520.37
-   **Minimum:** −3557.18
-   **Maximum:** −51.02

The observed scores were unusually negative.

An implementation audit of ELFEN 1.3.1 showed that the installed
implementation uses the syllable count divided by the number of
sentences in the Flesch calculation. Reproducing the calculation from
the installed implementation generated the same negative result observed
in the DataFrame.

For the first sentence, the standard calculation using the displayed
token, sentence, and syllable counts gives approximately **114.07**,
whereas the installed ELFEN implementation produces **−907.175**.

This indicates an implementation discrepancy between the installed ELFEN
calculation and the standard Flesch Reading Ease formulation.

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

## Key Conclusions

The experiment shows that ELFEN provides broad linguistic feature
coverage but that reproducibility depends on the availability and
coverage of external linguistic resources.

The main observations are:

1.  ELFEN documents **1,061 features across 11 linguistic areas**.
2.  **Morphology dominates the documented inventory with 798 features**.
3.  The experiment produced a final DataFrame with **1,106 columns**.
4.  **WordNet and NRC resources were necessary** for complete semantic
    and emotion-related extraction.
5.  Isolated feature groups showed different levels of missingness
    because of incomplete external resource coverage.
6.  After installing the required resources, the full pipeline completed
    with **0% missing cells**.
7.  The installed ELFEN 1.3.1 implementation produced unusually negative
    Flesch Reading Ease scores that could be reproduced from its
    implementation.

## Limitations

-   The evaluation uses a **1,000-sentence English dataset**, so the
    observations should not automatically be generalized to other
    languages or datasets.
-   External-resource coverage can vary by vocabulary and resource
    version.
-   The 1,106-column DataFrame includes base and helper/output columns
    and therefore is not directly equivalent to ELFEN's documented
    1,061-feature inventory.
-   The readability audit focuses specifically on the installed ELFEN
    1.3.1 implementation and the experimental configuration used in this
    project.

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

The experiment should record the package versions and external resource
versions because resource availability and coverage can affect the
output.

## Poster Sections

The poster is organized around:

-   **Hypothesis / Research Questions**
-   **Methodology**
-   **Results**
    -   Feature coverage and experimental output
    -   External resource dependencies
    -   Missingness
    -   Flesch Reading Ease and implementation discrepancy
-   **Limitations / Discussion**
-   **References**

## Main Poster Figures

The Results section uses:

1.  **Feature Distribution Across 11 Linguistic Areas**
2.  **ELFEN Experimental Output: 1,106 DataFrame Columns**
3.  **External Resource Dependencies & Missingness**
4.  **Distribution of Flesch Reading Ease Scores**

## Academic Note

The results reported here describe the specific ELFEN version,
resources, configuration, and dataset used in this experiment. Numerical
results should therefore be interpreted as experimental observations
from this setup rather than as universal properties of ELFEN.
