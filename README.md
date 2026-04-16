# Borzoi-Inspired Splice Site Prediction 

##Done by Soumi Sharma and Atrayee Samanta

## Overview
This repository contains our course project based on the **Borzoi** deep learning architecture for RNA expression and splicing prediction. The goal of this project was to read and synthesize the original Borzoi paper, and subsequently design our own custom deep learning model inspired by its methodology. 

Our project focuses specifically on predicting RNA splice sites. We built a 3-layer Convolutional Neural Network (CNN) and performed downstream interpretability analyses—specifically **In Silico Mutagenesis (ISM)** and **Variant Effect Prediction (VEP)**—mirroring the analyses shown in Figures 3-5 of the Borzoi paper.

## Repository Contents
- `ds202project.ipynb`: The primary Jupyter Notebook containing the data extraction pipeline, model architecture, training loop, and evaluation (including ISM and VEP analysis).
- `Slides`: A presentation summarizing the original Borzoi paper, our custom methodology, and the results of our implementation. 

## Dataset
- **Primary Source:** Human Chromosome 22 sequence data accessed via the Ensembl REST API.
- **Processing:** The pipeline extracts 400-base-pair (bp) sequence windows centered on true exon boundaries to serve as positive samples. Negative samples (decoys) are sampled from random genomic backgrounds that contain GT/AG motifs but are not located near annotated splice sites.
- **Dataset Size:** 62,220 total sequences (50% positive, 50% negative split).

## Model Architecture
We designed a lightweight CNN tailored for genomic sequence data, acting as a miniature version of the Borzoi convolutional tower:
- **Input:** 400bp One-hot encoded DNA sequences.
- **Layers:** Three 1D Convolutional layers equipped with Batch Normalization, GELU activations, and Max Pooling.
- **Head:** Adaptive 1D Max Pooling followed by fully connected linear layers with Dropout.
- **Total Parameters:** 103,489

## Results
The model was trained on 49,776 sequences and evaluated on a held-out test set of 12,444 sequences. It achieved robust predictive performance:
- **Test AUC:** 0.9471
- **Test Average Precision (AP):** 0.9498
- **Overall Accuracy:** 88.89%

## Interpretability & Variant Analysis
To connect our original work back to the Borzoi paper, we implemented:
- **In Silico Mutagenesis (ISM):** Mutating the center of the sequences (e.g., modifying the canonical GT donor site) to evaluate the model's structural reliance on these biological motifs.
- **Variant Effect Prediction (VEP):** Testing 75 specific genetic variants to observe how our model's predictions shift. This demonstrates the model's ability to evaluate the impact of single nucleotide polymorphisms (SNPs) on splicing mechanisms.

## Usage
1. Clone the repository to your local machine.
2. Install the necessary dependencies: `pip install torch numpy pandas matplotlib scikit-learn`
3. Open `ds202project.ipynb` in Jupyter Notebook or Google Colab.
4. Run the notebook to fetch the Ensembl data, train the model, and reproduce the ISM/VEP results. 

*(Note: Data fetching from Ensembl takes some time on the first run, but sequence data and exon coordinates are automatically cached locally for much faster subsequent runs).*
