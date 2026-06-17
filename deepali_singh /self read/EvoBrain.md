# Evo Brain: Continual Learning of EEG Foundation Models Across Heterogeneous BCI Tasks

**Authors:** Yangxuan Zhou, Sha Zhao, Jiquan Wang, Shijian Li, Gang Pan

---

# Abstract

## Background
EEG is one of the most widely used non-invasive BCI technologies because of:
- High temporal resolution
- Low cost
- Ease of acquisition
- Clinical applicability

Traditional EEG systems require separate models for:
- Sleep Staging
- Motor Imagery
- Emotion Recognition
- Mental Disorder Diagnosis

### Main Problem
Lack of generalization and scalability due to task-specific fine-tuning.

---

# Proposed Solution: EvoBrain

EvoBrain is a continual learning framework for EEG foundation models.

Instead of:

Task A → Separate Model
Task B → Separate Model
Task C → Separate Model

EvoBrain enables:

Task A
↓
Task B
↓
Task C
↓
One Evolving EEG Model

---

# Key Components

## Neuro-Spectral Task Normalization (NSN)
Handles:
- Distribution shifts
- Spectral differences across tasks

## Response-Affinity Distillation (RAD)
Handles:
- Knowledge retention
- Transfer learning
- Catastrophic forgetting

---

# Introduction

## What is a BCI?

Brain Activity
↓
Computer Interpretation
↓
External Device Control

BCIs convert neural activity into machine commands.

---

# Evolution of EEG Decoding

## Stage 1: Traditional Methods
- CSP
- SVM
- LDA

Limitations:
- Manual feature engineering
- Poor scalability

## Stage 2: Deep Learning
- CNN
- RNN
- LSTM
- Transformers

Advantages:
- Automatic feature extraction
- Improved performance

Limitation:
- Still task-specific

## Stage 3: EEG Foundation Models

Massive EEG Dataset
↓
Self-Supervised Pretraining
↓
Foundation Model
↓
Task-Specific Fine-Tuning

Problem:
Separate model required for each task.

---

# Major Challenges

## Plasticity
Ability to learn new tasks.

## Stability
Ability to retain knowledge from previous tasks.

### Plasticity–Stability Trade-off

Too Much Plasticity
↓
Forget Old Tasks

Too Much Stability
↓
Cannot Learn New Tasks

---

# Contributions

1. Introduces EvoBrain for cross-task continual learning.
2. Introduces NSN and RAD.
3. Works across:
   - CBraMod
   - EEGMamba
   - CSBrain
   - DeeperBrain

without modifying architecture.

---

# Related Work

## EEG Decoding

EEG
↓
Handcrafted Features
↓
Machine Learning
↓
Prediction

Problems:
- Subject sensitivity
- Poor generalization

## EEG Foundation Models

Massive EEG Dataset
↓
Pretraining
↓
Foundation Model
↓
Fine-Tuning

Still requires separate models.

## Continual Learning Methods

- EWC
- LwF
- Replay Methods
- Parameter Isolation

Research gap:
Previous work focused on subject-level adaptation, not task-level continual learning.

---

# Methodology

## Task Sequence

T1
T2
T3
...
TN

Example Tasks:
- Sleep Staging
- Motor Imagery
- Emotion Recognition
- Speech Recognition
- Depression Diagnosis

Goal:
Create one EEG foundation model that learns all tasks sequentially.

---

# EvoBrain Architecture

Heterogeneous EEG Tasks
↓
Pretrained EEG Foundation Model
↓
Neuro-Spectral Task Normalization
↓
Time-Dependent Replay
↓
Response-Affinity Distillation
↓
Updated Foundation Model

---

# Heterogeneous Tasks

The framework continuously learns:
- Sleep Staging
- Motor Imagery
- Emotion Recognition
- Imagined Speech
- Mental Disorder Diagnosis

using a single evolving backbone.

---

# Conclusion

## Main Achievement

EvoBrain is the first framework for cross-task continual learning in EEG foundation models.

### Benefits
- Knowledge sharing across tasks
- Reduced retraining cost
- Improved scalability
- Better balance between plasticity and stability

### Final Takeaway

EvoBrain enables a single EEG foundation model to continuously learn multiple heterogeneous BCI tasks while retaining previously learned knowledge.
