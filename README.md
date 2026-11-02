# PTB-XL ECG Classification Project

This repository focuses on multi-label diagnostic classification of electrocardiogram (ECG) data utilizing the publicly available [PhysioNet PTB-XL Database](https://physionet.org/content/ptb-xl/1.0.3/).

## Dataset & Preprocessing
- **Source:** [PTB-XL clinical ECG database (v1.0.3)](https://physionet.org/content/ptb-xl/1.0.3/)
- **Signals:** 12-lead ECG recordings at 100 Hz.
- **Preprocessing:** Baseline wander and high-frequency noise removal using a $0.5	ext{--}50$ Hz 4th-order butterworth bandpass filter.
- **Splitting Strategy:** Strict patient-level stratification following recommended benchmark splits: Folds 1-8 for training, Fold 9 for validation, and Fold 10 for testing.

## Models & Architectures
1. **SVM Classical ML Baseline**
   - Extracted 48 features (Mean, Variance, Skewness, Spectral FFT energy across 12 leads).
   - Cleaned missing statistics using Mean Imputation.
   - Multi-label prediction handled via `OneVsRestClassifier` wrapping an RBF kernel Support Vector Classifier.
2. **Deep Learning (1D-CNN)**
   - Four-block 1D-CNN using sequential convolutional downsampling, batch normalization, ReLU, max pooling, and Global Average Pooling to classify the raw temporal signals directly.
3. **Hybrid CNN-RNN (GRU)**
   - A combined model utilizing a shallow CNN feature extractor to compress 1000 ECG sequence steps down to 125, followed by a stacked Bidirectional GRU layer with max-temporal reduction to resolve sequence memory bottlenecks.

## Diagnostic Superclasses
Models classify patients into 5 disjoint diagnostic categories:
- **CD:** Conduction Disturbance
- **HYP:** Hypertrophy
- **MI:** Myocardial Infarction
- **NORM:** Normal ECG
- **STTC:** ST/T Change

## Results and Performance (Tested on Fold 10)
| Model Architecture | Micro F1-Score | Macro F1-Score |
| :--- | :---: | :---: |
| SVM Feature Baseline | 0.5561 | 0.4242 |
| End-to-End 1D-CNN | 0.7535 | 0.7041 |
| Hybrid CNN-GRU | 0.7647 | 0.7268 |

## Project Layout
- `requirements.txt`: Specified Python dependencies.
- `README.md`: Summary of project scope and metrics.
