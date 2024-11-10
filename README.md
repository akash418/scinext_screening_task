# Virology and Epidemiology Research Paper Filtering and Classification

## Introduction

This project is a Natural Language Processing (NLP) solution for curating and analyzing research papers in the fields of virology and epidemiology. The aim is to streamline the process of finding, filtering, and categorizing relevant academic papers using deep learning techniques. Given a list of scientific papers, this solution identifies and labels papers based on specific criteria, reducing the need for manual keyword filtering and improving classification accuracy. The code can be run on Google colab: T4 GPU hardware.

## Purpose and Usage

The solution automates three key tasks:

1. **Semantic Filtering** – Filtering out irrelevant papers based on the semantic similarity of the abstract and title to the term "deep learning."
2. **Method Classification** – Classifying relevant papers by the type of method used: computer vision, text mining, both, or other.
3. **Method Identification** – Identifying scientific methods mentioned in relevant papers' abstracts.

This repository includes code for each task, with outputs available in the dataset as additional columns, including `Label` (the paper's method classification) and `Top_Methods` (scientific methods identified in each paper). Each section below outlines the methodology and reasoning behind the approach taken.

## Contents of this Repo
- filtered_papers.csv - CSV containing the filtered papers with the appropriate label and the identified scientific methods
- scinext_screening.ipynb -  Notebook containing code
- scinext_screening_report.pdf - Detailed report containing the apprach to the solution and some intresting findings.


## Solution Steps and Findings

### Task #1: Semantic Filtering of Papers

Using a sentence transformer model, the solution measures semantic similarity between each paper's (title + abstract) and the phrase "deep learning." Papers exceeding a threshold similarity score (determined through manual inspection) are marked as relevant.

**Advantages of Semantic Filtering Over Keyword Matching**:
- Enhanced contextual understanding of abstracts, capturing synonyms and related phrases.
- Better adaptability to complex, scientific text.
- Reduced need for continuous updates to keyword lists, unlike lexicon-based approaches.

After implementing this approach, 7,927 papers were identified as relevant.

### Task #2: Method Classification of Relevant Papers

To categorize each relevant paper by method type, a zero-shot classification approach was applied using a BART model. The model was fine-tuned to classify texts into specific labels—computer vision, text mining, both, or other—based on a calculated confidence score for each label.

**Alternative Approaches Explored**:
- Semantic similarity with keyword matching was tested but lacked scalability, as constructing exhaustive keyword lists for both domains proved challenging.

### Task #3: Identification of Scientific Methods

The most complex task involved extracting specific scientific methods from paper abstracts. A token classification approach using a SciBERT model was chosen. The model was fine-tuned to recognize tokens according to the BIO scheme, enabling effective extraction of complex scientific terms.

**Processing and Post-Processing**:
- To handle BERT’s sequence length limit (512 tokens), abstracts were chunked as needed.
- The tokens were then passed through a helper function to refine and clean the extracted terms, resulting in accurate method identification.

**Alternative Approaches Explored**:
- **LDA Topic Analysis** – Limited alignment with expected scientific terms and difficulties in estimating the number of topics and keywords for this dataset.
- **TF-IDF** – Ineffective at distinguishing scientific methods from other terms without a reliable heuristic.

## Columns Added to the Dataset

- **`Label`**: The category assigned to each relevant paper, indicating the main method it employs.
- **`Top_Methods`**: A list of scientific methods identified in each paper’s abstract.

## Conclusion

This project demonstrates a robust NLP pipeline for filtering and classifying scientific literature in specialized domains. The semantic filtering and deep learning-based classification approach enhance relevance and precision, reducing the limitations of keyword-based methods. The final code is well-documented and suitable for adapting to similar tasks in other research fields