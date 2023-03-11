# Do Tissue Source Sites leave identifiable Signatures in Whole Slide Images beyond staining?
### Piotr Keller*, Muhammad Dawood* and Fayyaz ul Amir Afsar Minhas
### Tissue Image Analytics Center, University of Warwick, United Kingdom

This repository contains the code for the following manuscript:

Do Tissue Source Sites leave identifiable Signatures in Whole Slide Images beyond staining?, accepted by ICLR 2023 Workshop TML4H.

## Introduction
Why can deep learning predictors trained on Whole Slide Images fail to generalize? It is a common theme in Computational Pathology (CPath) to see a high performing model developed in a research setting experience a large drop in performance when it is eventually deployed to a new clinical environment. One of the major reasons for this is the batch effect that is introduced during the creation of whole slide images resulting in a domain shift. CPath pipelines try to reduce this effect via stain normalization techniques. However, in this paper, we provide empirical evidence that stain normalization methods do not result in any significant reduction of the batch effect. This is done via clustering analysis of the dataset as well as training weakly-supervised models to predict source sites. This study aims to open up avenues for further research for effective handling of batch effects for improving trustworthiness and generalization of predictive modelling in the Computational Pathology domain.

<img src="Staining.png" alt="Block Diagram"/>

## Dependencies
scipy 1.7.3

numpy 1.21.6

matplotlib 3.2.2

geomloss 0.2.4

pandas 1.3.5

torch 1.12.1+cu113

sksurv 0.17.2

lifelines 0.27.4

sklearn 1.0.2

seaborn 0.11.2

tqdm 4.64.1 

## Usage
### Step 1. Data download
Download the FFPE whole slide images from GDC portal (https://portal.gdc.cancer.gov/) for breast carcinoma (TCGA-BRCA).

Download corresponding gene point mutation and Disease Specific Survival from cBioPortal (https://www.cbioportal.org/).
### Step 2. Data processing
Using the code under `code_data_processing` to perform

- Slide selection: select high quality WSIs from the original dataset 
- Tile extraction: extract 512x512 tiles from the large WSI at a spatial resolution of 0.50 microns-per-pixel
- Patches capturing less that 40% of informative tissue are discarded
- Stain normalization
- Feature extraction: extract a feature vector for each tile using SuffleNet pretrained on ImageNet


Details can be found in the paper and code_data_processing.
### Step 3. Training and evaluation of CLAM model with 1024-dimensional feature representations of 1052 TCGA-BRCA slides 

Used the code from [`Clustering-constrained attention multiple instance learning (CLAM)`](https://github.com/mahmoodlab/CLAM) Github to train a neural network for predicting source site labels.

Details about the model can be found in the orginal [paper](https://arxiv.org/abs/2004.09666).

### Step 4. MMD Kernel generation for 1024-dimensional feature representations of 1052 TCGA-BRCA slides 

Using the code from [`Maximum Mean Discrepancy Kernels For Predictive And Prognostic Modeling Of Whole Slide Images`](https://github.com/engrodawood/Hist-MMD) Github to generate an $N \times N$ distance matrix using MMD where $N$ is the number of WSIs in a dataset.

Details can be found in the original [paper](https://arxiv.org/abs/2301.09624).

### Step 5. Source site prediction using MMD kernels

Using the code from `source_site_prediction.py` to generate a Support Vector Machine (SVM) with a predefined kernel (generated from Step 4.) to predict source site origin of a given Whole Slide Image.

Details can be found in the paper.

## Note

Some intermediate data are put into the folder `data`.

--------

\* Joint first authorship
