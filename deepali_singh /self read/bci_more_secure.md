# Making Brain-Computer Interfaces More Secure

**Authors:** Md Fahimul Kabir Chowdhury, Gahangir Hossain

---

# Paper Overview

## Research Problem

Most EEG-based Brain-Computer Interfaces (BCIs) focus on:
- Classification Accuracy
- Feature Extraction
- Deep Learning Performance

Very little work focuses on:
### Security of BCIs

The paper investigates:
- Can adversarial attacks fool EEG-based BCIs?
- Can robust CNN architectures resist such attacks?

---

# ABSTRACT NOTES

## Background
BCIs use EEG signals and Machine Learning to interpret human intentions.

### Problem
Adversarial attacks add tiny perturbations that:
- Are nearly invisible
- Cause wrong predictions
- May lead to incorrect diagnosis

## Authors' Contribution
A lightweight custom CNN is proposed and compared with:
1. EEGNet
2. DeepConvNet
3. SleepEEGNet

## Main Result
The proposed CNN remains highly robust under adversarial attacks.

---

# INTRODUCTION

## What is a BCI?

Human Brain
↕
Computer / External Device

Direct communication without muscles or traditional input devices.

## Why EEG?
Advantages:
- Non-invasive
- Low cost
- Portable
- Easy to use

## EEG-Based BCI Architecture

Brain Signals
→ Signal Acquisition
→ Signal Preprocessing
→ Machine Learning
→ Control Execution
→ External Device

---

# ADVERSARIAL ATTACKS

## Types
### White Box Attack
Attacker knows:
- Architecture
- Parameters
- Gradients

### Gray Box Attack
Partial knowledge.

### Black Box Attack
No model knowledge.

The paper focuses on White Box attacks.

---

# FGSM ATTACK

## Fast Gradient Sign Method

Original EEG
→ Compute Gradient
→ Add Perturbation
→ Adversarial EEG
→ Wrong Prediction

Formula:

x_adv = x + ε · sign(∇J)

---

# PGD ATTACK

## Projected Gradient Descent

Stronger version of FGSM.

Repeated optimization makes PGD significantly stronger than FGSM.

---

# METHODOLOGY

## Goal
Develop a CNN that remains accurate even under adversarial attacks.

### Pipeline

EEG Signals
→ Preprocessing
→ Spectrogram Generation
→ CNN Training
→ FGSM / PGD Attack
→ Robustness Evaluation

## Data Preprocessing
- Normalization: 0–1
- Resize: 224 × 224

## Cross Validation
Stratified K-Fold (K = 10)

---

# PROPOSED CNN

Input (224 × 224 × 3)
→ Conv2D (8)
→ BatchNorm
→ Conv2D (16)
→ BatchNorm
→ MaxPooling
→ Conv2D (32)
→ BatchNorm
→ AveragePooling
→ Flatten
→ Dropout (0.25)
→ Dense
→ Softmax

---

# DATASETS

## MI4 Dataset
- 9 subjects
- 22 channels
- 250 Hz

## rTMS Dataset
- 15 depression patients
- 19 channels
- 500 Hz

---

# RESULTS

## Baseline Accuracy

| Model | Accuracy |
|--------|----------|
| SleepEEGNet | 24.91% |
| EEGNet | 42.35% |
| DeepConvNet | 47.66% |
| Proposed CNN | 88.21% |

## After FGSM Attack

| Model | Accuracy |
|--------|----------|
| SleepEEGNet | 1.36% |
| EEGNet | 5.27% |
| DeepConvNet | 6.17% |
| Proposed CNN | 73.02% |

---

# CONCLUSION

1. EEG-based BCIs are vulnerable to adversarial attacks.
2. Lightweight CNNs can be robust.
3. Security should be evaluated alongside accuracy.

### Final Takeaway
Future BCI systems should optimize:
- Accuracy
- Robustness
- Security
- Reliability
