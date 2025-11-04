EEG Seizure Detection Using Classical and Deep Learning Methods
🧠 Overview

This project develops a pipeline for automatic seizure detection from multichannel EEG signals.
It combines classical machine learning (PCA, CSP, SVM, XGBoost) with deep learning (CNN) to analyze spatial and temporal brain patterns and identify seizure events.

📊 Dataset Description

Total segments: 3546 EEG samples

Channels: 23

Timepoints per sample: 256

Labels: Seizure / Non-seizure

Each segment represents a short EEG window, sampled at 256 Hz.

⚙️ Preprocessing

Noise filtering: Bandpass filter (1–70 Hz)

Standardization: Mean–variance normalization per channel

Dimensionality reduction: PCA applied after flattening each EEG sample (23 × 256 → 5888 features)

🧩 Methods and Pipelines
1. PCA and Logistic Regression

PCA reduced the 5888-dimensional data to 302 components (90% variance).

Logistic Regression on top of PCs achieved ~53.5% accuracy.

PCA visualization revealed overlapping classes but meaningful temporal-spatial trends.

2. Covariance + PCA + SVM

Built 23 × 23 covariance matrices per sample to capture spatial relationships.

PCA reduced these features, and an SVM classifier achieved AUC = 0.763.

PCA components revealed seizure-related global correlations.

3. GModPCA + SVM

PCA applied individually per channel, then combined.

Achieved AUC = 0.751 with 2 components per channel.

4. ICA-GMod + XGBoost

Independent Component Analysis (ICA) extracted independent temporal sources per channel.

SVM achieved AUC = 0.748; replacing it with XGBoost improved to AUC = 0.867.

Demonstrated the power of nonlinear models for EEG data.

5. Hybrid CNN + Statistical Features

EEG reshaped to (23 × 256 × 1) and fed into a CNN.

Extracted 5 statistical features per channel: mean, std, max, min, and power.

CNN captured spatio-temporal dependencies.

Achieved 88.45% accuracy and 0.9520 ROC AUC — the best among all models.

🧭 Interpretability & Analysis
🧠 Spatial Importance (Occlusion Analysis)

Each EEG channel was zeroed out to test its impact on accuracy.

Most informative channels: 19, 21, 15, 2

Least informative channels: 7, 5, 8, 17

🕒 Temporal Discrimination

A sliding window (size = 64 samples) revealed that timepoints 50–150 are most informative for seizure detection.

🔥 CSP + SVM Pipeline

Common Spatial Pattern (CSP, 4 components) → StandardScaler → SVM (RBF kernel).

Achieved 80.14% accuracy and 81.42% F1-score.

CSP heatmaps showed highest contribution from channels 9 and 16.

Temporal CSP analysis confirmed discriminative activity in the middle of the trial (~sample 128)
