# **Atoms of Thought: Universal EEG Representation Learning with Micro states**

---

# 1. Paper Overview

## Authors

- Xinyang Tian
- Ruitao Liu
- Ziyi Ye
- Siyang Xue
- Xin Wang
- Xuesong Chen

---

# Main Idea

The paper proposes a new way of representing EEG signals using **EEG Microstates** instead of traditional:

1. Time-domain EEG signals
2. Frequency-domain EEG features

The authors claim that EEG microstates act as:

> "Atoms of Thought"
> 

because they represent the smallest stable patterns of brain activity.

---

# Motivation

Traditional EEG features suffer from:

### Problem 1: Noise Sensitivity

EEG contains:

- Eye blink artifacts
- Muscle artifacts
- Environmental noise

which reduce performance.

---

### Problem 2: Poor Generalization

EEG varies greatly:

- Between subjects
- Between tasks

Example:

```
Person A EEG ≠ Person B EEG
Sleep EEG ≠ Emotion EEG
```

Hence models trained on one dataset often fail on another.

---

### Problem 3: Information Loss

### Time-domain Features

Use raw EEG.

Problem:

```
Low Signal-to-Noise Ratio
```

---

### Frequency-domain Features

Use FFT/STFT.

Problem:

```
Windowing
      ↓
Reduced Temporal Resolution
      ↓
Information Loss
```

---

# Proposed Solution

The authors introduce:

# EEG Microstates

Microstates are short-lasting stable brain states.

Duration:

```
60 – 120 ms
```

Each microstate corresponds to a unique brain activity pattern.

---

# Core Hypothesis

Instead of learning:

```
Raw EEG
```

or

```
Frequency Spectrum
```

learn:

```
Sequence of Brain States
```

which is biologically meaningful.

---

# Abstract Notes

The paper proposes:

## Universal Microstate Tokenizer

Large EEG dataset

↓

Clustering

↓

Discrete Microstates

↓

Universal Representation

↓

Downstream Tasks

The same tokenizer is used for:

- Sleep Staging
- Emotion Recognition
- Motor Imagery

Results show:

- Better accuracy
- Better scalability
- Better interpretability

than traditional representations.

---

# 2. Introduction

## Why EEG?

EEG is important because it helps in:

### Clinical Applications

- Epilepsy detection
- Sleep disorder diagnosis
- Alzheimer's studies

### Neuroscience

- Cognitive research
- Attention studies
- Emotion analysis

### Brain Computer Interfaces (BCI)

- Device control
- Motor imagery decoding

---

# Traditional EEG Learning

```
Raw EEG
   ↓
Feature Extraction
   ↓
Machine Learning
   ↓
Prediction
```

Problems:

- Subject dependence
- Task dependence
- Poor generalization

---

# Figure 1 Explained

(Page 2)

The paper compares three EEG representations.

https://images.openai.com/static-rsc-4/VpQ1XTIG_1MdRRPVFmzKCni2cjVk1BvZyq0ggMDWH6xcGV0HzeU8LzcomoFOmS4xqs9uj76ObYux87mH68FmRZRvZizv6X7KHfoBa09sYU2NB_G8CNy4IgJtuslEqUM3DEwmv-5CD_ND13Pzyyn_h4TNRwFd-bbUvd3Fl8kL6vkZrt_5xT-l6ZgZdfDjb3P4?purpose=fullsize

https://images.openai.com/static-rsc-4/nc7dW3eV-0WUezywskPtCp6buBULR0Z8Jsc-LsAk3ttpmtPzC1yCFWhvnWkdoskIoD9bFppNjjEoxJHTGCHmVsNY-tshPL0izc659R9k1q1IWQlJijKoYgiAWoqJo2ZZ8KRxMn4iXITMLNADUhA6XkBMmh0HrIc_r6IzlpSdtrrTqRdGHo3UK8V_eP494cfC?purpose=fullsize

https://images.openai.com/static-rsc-4/QnVJK9_k4EyWY-jzc2yc0380Uwz7jKW3tXUVLcXl6gEVe9HkztHlrjfh-3MonsXjIuiZ4brygzQ-ukahKoGOon0blMK3nSiQudUa8B55XVfEh_P6_tbGaqxfRF8PzVsXf91vAjBw8HdZuPMZyV517ppvxpXSSUn_lAVepp7o1BFhPCYwqdkISWAMZuFRQENp?purpose=fullsize

6

### Time Domain

Uses raw EEG waveform.

---

### Frequency Domain

Uses:

- STFT
- Frequency band powers

---

### Microstates (Proposed)

Uses:

```
EEG
 ↓
Clustering
 ↓
Discrete Brain States
 ↓
Learning
```

The same representation works across:

```
Sleep Staging
Emotion Recognition
Motor Imagery
```

---

# Main Contributions

## Contribution 1

Introduces EEG microstates as a universal EEG representation.

---

## Contribution 2

Shows microstates outperform:

- Raw EEG
- STFT Features

on multiple tasks.

---

## Contribution 3

Shows microstates scale better with larger datasets.

---

## Contribution 4

Provides explainability by linking microstates to cognitive states.

---

# 3. Related Work

## EEG Microstates in Neuroscience

Microstates were introduced by:

### Lehmann et al. (1987)

Idea:

Brain activity consists of short stable patterns.

---

## Applications of Microstates

Researchers found changes in microstates in:

### Diseases

- Epilepsy
- Sleep disorders
- Alzheimer's Disease

### Cognitive Tasks

- Attention
- Emotion
- Social cognition

---

# Research Gap

Previous studies:

```
Microstates
      ↓
Interpretation
```

not

```
Microstates
      ↓
Deep Learning
      ↓
Classification
```

This paper fills that gap.

---

# 4. EEG Representations

The paper compares three representations.

---

# A. Time Domain Representation

Uses raw EEG directly.

Input:

```
EEG
(C × fs × T)
```

where

C = channels

fs = sampling frequency

T = duration

---

Pipeline:

```
Raw EEG
   ↓
Windowing
   ↓
Deep Learning
```

---

# B. Frequency Domain Representation

Uses Short-Time Fourier Transform (STFT).

Pipeline:

```
Raw EEG
   ↓
STFT
   ↓
Frequency Bands
   ↓
Band Power Features
```

---

# EEG Frequency Bands

| Band | Range |
| --- | --- |
| Delta | 0.5–4 Hz |
| Theta | 4–8 Hz |
| Alpha | 8–12 Hz |
| Sigma | 12–16 Hz |
| Beta | 16–30 Hz |
| Gamma | 30–40 Hz |

---

# Why STFT?

Provides:

```
Time + Frequency Information
```

But suffers from:

```
Fixed Window
      ↓
Loss of Temporal Detail
```

---

# C. Microstate Representation (Proposed)

This is the heart of the paper.

---

# What is a Microstate?

A microstate is:

> A stable scalp potential pattern lasting 60–120 ms.
> 

Think of it as:

```
Brain Frame 1
Brain Frame 2
Brain Frame 3
...
```

similar to frames in a video.

---

# Microstate Tokenizer

The authors build a tokenizer similar to NLP tokenizers.

---

# Stage 1: Dataset Selection

Used:

### Human Sleep Project (HSP)

Contains:

```
20,000+ Subjects
```

Large-scale sleep EEG dataset.

---

# Why Sleep Data?

Because:

1. Huge dataset available
2. Contains rich brain activity
3. Tests generalization to wake tasks

---

# Stage 2: Channel Selection

Selected 6 common channels:

```
F3
F4
C3
C4
O1
O2
```

---

# Stage 3: Filtering

Band-pass filter:

```
1 Hz – 40 Hz
```

removes noise.

---

# Stage 4: Resampling

All signals converted to:

```
100 Hz
```

---

# Stage 5: Global Field Power (GFP)

GFP = standard deviation across channels.

High GFP peaks correspond to:

```
Strong Brain States
```

Only GFP peaks are retained.

---

# Stage 6: Clustering

Used:

### Streaming K-Means

Advantages:

- Memory efficient
- Handles huge datasets

Parameters:

```
Clusters (k) = 1000
Batch Size = 50
```

---

# Result

Creates:

```
Microstate Vocabulary
```

of

```
1000 Brain Tokens
```

---

# Figure 2 Explained

Pipeline:

```
Sleep EEG
      ↓
Filtering
      ↓
Resampling
      ↓
GFP Peaks
      ↓
Streaming K-Means
      ↓
1000 Microstates
      ↓
Microstate Codebook
```

Then:

```
Microstate Sequence
      ↓
Embedding Layer
      ↓
Neural Network
      ↓
Classification
```

---

# 5. Downstream Tasks

The same tokenizer is used for three tasks.

---

# Task 1: Sleep Staging

Classes:

```
W
N1
N2
N3
REM
```

---

Sleep Stages:

```
Wake
 ↓
N1
 ↓
N2
 ↓
N3 (Deep Sleep)
 ↓
REM
```

---

Models Used

### CNN + LSTM

- CNN → spatial patterns
- LSTM → temporal patterns

---

### Sleep Transformer

Uses Transformer architecture.

---

### SleepNetZero

Residual + RoFormer architecture.

---

# Sleep Staging Results

| Representation | Accuracy |
| --- | --- |
| Raw EEG | 79.3% |
| STFT | 79.4% |
| Microstates | 81.0% |

Best Result:

```
Microstates = 81%
```

---

# Task 2: Emotion Recognition

Dataset:

### SEED

Emotions:

```
Positive
Neutral
Negative
```

---

Model:

CNN Classifier

---

# Results

| Representation | Accuracy |
| --- | --- |
| Raw EEG | 84.6% |
| STFT | 79.7% |
| Microstates | 86.2% |

Microstates again achieve the highest performance.

---

# Task 3: Motor Imagery Classification

Classes:

```
Left Hand
Right Hand
Both Hands
Both Feet
```

---

# Results

| Representation | Accuracy |
| --- | --- |
| Raw EEG | 36.2% |
| STFT | 32.3% |
| Microstates | 43.7% |

Largest improvement among all tasks.

---

# 6. Key Findings

## Finding 1

Microstates outperform both:

```
Time Domain
Frequency Domain
```

across all tasks.

---

## Finding 2

Microstates are universal.

Tokenizer trained on:

```
Sleep EEG
```

works on:

```
Emotion Recognition
Motor Imagery
```

without retraining.

---

## Finding 3

Microstates scale better.

As dataset size increases:

```
More Data
      ↓
Greater Accuracy Gain
```

compared to other features.

---

## Finding 4

More microstates improve performance.

```
100
 ↓
500
 ↓
1000
 ↓
Better Results
```

---

# 7. Interpretability Analysis

One of the strongest parts of the paper.

---

# Sleep Stage Analysis

Researchers examined the most frequent microstates.

---

# Wake (W) and REM

Shared microstates:

```
419
161
```

Reason:

```
REM ≈ Wake-like Brain Activity
```

Dreaming occurs during REM.

---

# N3 Deep Sleep

Frequent microstates:

```
378
452
487
651
```

These correspond to:

```
High Amplitude Delta Waves
```

which are characteristic of deep sleep.

---

# Figure 5 Interpretation

```
W Stage
   ↑
 Similar
   ↓
REM Stage

N3 Stage
   ↓
Different Microstates
```

This proves microstates:

✔ Capture similarities

✔ Preserve differences

simultaneously.

---

# 8. Advantages of Microstates

## Better Robustness

Less sensitive to:

- Noise
- Artifacts

---

## Better Generalization

Works across:

- Subjects
- Tasks

---

## Better Interpretability

Links directly to:

- Cognitive states
- Brain activity

---

## Better Scalability

Performance improves with larger datasets.

---

# 9. Limitations

The authors mention:

### Limitation 1

Only 3 tasks tested.

---

### Limitation 2

Tokenizer trained only on sleep EEG.

---

### Limitation 3

No pre-trained EEG foundation model built yet.

---

# Future Work

The authors suggest: