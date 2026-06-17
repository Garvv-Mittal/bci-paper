# **A Domain-Informed Multi-Objective Framework for EEG Channel Selection in Motor Imagery BCIs**

**Authors:** Dekka Muni Kumar, Dhruba Jyoti Kalita, Yogesh Kumar Meena 

### **ABSTRACT**

## Research Problem

Motor Imagery (MI)-based Brain Computer Interfaces (BCIs) depend heavily on EEG signals.

A major challenge is:

**Selecting the most relevant EEG channels.**

Traditional methods suffer from:

- Single-objective optimization
- Local optima
- Fixed channel subsets
- Poor balance between accuracy and physiological relevance

---

## Proposed Solution

The authors propose a:

### Domain-Informed Multi-Objective Optimization Framework

using:

1. NSGA-II
2. MOPSO
3. MOEA/D

The framework simultaneously optimizes:

### Objective 1

Spatial Relevance

### Objective 2

Functional Discriminability

instead of combining them into a single score.

---

## Main Results

The framework was tested on:

- Physionet
- OpenBMI
- HighGamma
- BCIIV-2A

Classification accuracies achieved:

| Dataset | Accuracy |
| --- | --- |
| Physionet | 87% |
| OpenBMI | 71% |
| HighGamma | 75% |
| BCIIV-2A | 65% |

Benefits:

✔ Better classification

✔ Compact channel subsets

✔ Lower computational complexity

✔ Suitable for wearable BCIs

✔ Suitable for real-time BCIs

---

# 1. INTRODUCTION

## What is Motor Imagery (MI)?

Motor Imagery means:

A person imagines movement without physically moving.

Example:

- Imagine moving left hand
- Imagine moving right hand

These thoughts create EEG patterns that can be decoded by a BCI system.

---

## Applications of MI-BCI

### Medical

- Paralysis assistance
- Stroke rehabilitation
- Neuroprosthetics

### Robotics

- Robotic arm control
- Wheelchair navigation

### Human-Computer Interaction

- Cursor control
- Virtual reality interaction

---

## Importance of Channel Selection

EEG systems may contain:

- 22 channels
- 64 channels
- 128 channels

Not all channels contain useful information.

Problems of using all channels:

- Increased noise
- Redundant information
- High computational cost
- Difficult real-time deployment

Hence optimal channel selection is essential.

---

# Existing Channel Selection Methods

## 1. Statistical Methods

Use significance tests to rank channels.

Problem:

May ignore physiological meaning.

---

## 2. Filter-Based Methods

Rank channels using feature scores.

Problem:

Often optimize only one objective.

---

## 3. Domain Knowledge Methods

Focus on channels around:

- C3
- C4

which are located over motor cortex.

Problem:

May miss subject-specific information.

---

# Previous Multi-Objective Approaches

## LI-Based Methods

Use lateralization index.

Measure hemispheric activation.

---

## LRP-Based Methods

Use readiness potentials and NSGA-II.

---

## IMOCS

Iterative Multi-Objective Channel Selection.

Problem:

Combines multiple objectives into one score.

Trade-offs become hidden.

---

# Limitation Identified by Authors

Most existing approaches:

- Merge objectives
- Optimize a single criterion

This causes:

- Loss of trade-off information
- Suboptimal channel selection

---

# Proposed Idea

Treat:

### Spatial Relevance

and

### Functional Discriminability

as independent objectives.

Then solve using Pareto Optimization.

---

# Contributions

## Contribution 1

New multi-objective EEG channel selection framework.

---

## Contribution 2

Introduces Gaussian Kernel Spatial Relevance.

---

## Contribution 3

Introduces ITTRD Functional Discriminability.

---

## Contribution 4

Benchmarks on four major EEG datasets.

---

# 2. PROPOSED FRAMEWORK

Figure 1 of the paper shows the complete framework.

Pipeline:

```
EEG Data
   ↓
Preprocessing
   ↓
Channel Selection
   ↓
Feature Extraction
   ↓
Feature Selection
   ↓
SVM Classification
```

---

# Step 1: Preprocessing

Raw EEG signals are:

1. Segmented into trials
2. Filtered using

### 5th Order Butterworth Bandpass Filter

Frequency range:

4 – 40 Hz

Purpose:

Retain MI-related frequencies.

Remove:

- Noise
- Artifacts

---

# Step 2: Multi-Objective Channel Selection

The optimization problem is:

### Maximize

1. Spatial relevance
2. Functional discriminability

subject to:

Maximum selected channels ≤ L

where L = 16.

---

# Objective 1: Spatial Relevance

Idea:

Channels near motor cortex should be preferred.

Motor cortex reference channels:

### C3

### C4

Authors use a Gaussian kernel.

---

### Spatial Relevance Equation

fspk=exp⁡(−∣Cref−Ck∣22σ2)f_{sp}^{k}
=
\exp
\left(
-\frac{|C_{ref}-C_k|^2}{2\sigma^2}
\right)

fspk=exp(−2σ2∣Cref−Ck∣2)

fspk=exp⁡(−∣Cref−Ck∣22σ2)f_{sp}^{k}=\exp\left(-\frac{|C_{ref}-C_k|^2}{2\sigma^2}\right)fspk=exp(−2σ2∣Cref−Ck∣2)

Meaning:

Closer channels receive larger weights.

Far channels receive smaller weights.

---

# Objective 2: Functional Discriminability

Authors propose:

## ITTRD

Intratrial Task Related Desynchronisation

---

### Idea

Measure change in spectral power between:

- Baseline period
- Activation period

for every trial.

---

### ITTRD Equation

ITTRD=Pactivation−PbaselinePbaseline×100ITTRD=
\frac{P_{activation}-P_{baseline}}
{P_{baseline}}
\times100

ITTRD=PbaselinePactivation−Pbaseline×100

ITTRD=Pactivation−PbaselinePbaseline×100ITTRD=\frac{P_{activation}-P_{baseline}}{P_{baseline}}\times100ITTRD=PbaselinePactivation−Pbaseline×100

---

Interpretation:

### Negative ITTRD

ERD

(Event Related Desynchronization)

Power decreases.

---

### Positive ITTRD

ERS

(Event Related Synchronization)

Power increases.

---

# Why Combine Both Objectives?

Spatial relevance alone:

May select nearby but weak channels.

Functional relevance alone:

May select strong but irrelevant channels.

Combining both:

Produces physiologically meaningful channels.

---

# 3. OPTIMIZATION ALGORITHMS

The framework uses three algorithms.

---

# A. NSGA-II

### Full Form

Non-dominated Sorting Genetic Algorithm II

---

### Working

1. Randomly initialize channel subsets.
2. Evaluate objectives.
3. Tournament selection.
4. Crossover.
5. Mutation.
6. Non-dominated sorting.
7. Crowding-distance preservation.
8. Generate Pareto-optimal solutions.

---

### Advantage

Maintains diversity.

Avoids local optima.

---

# B. MOPSO

### Full Form

Multi-Objective Particle Swarm Optimization

---

### Working

Each particle:

= one channel subset.

Particle learns from:

- Personal best (pbest)
- Global best (gbest)

---

### Advantage

Fast convergence.

Explores large search spaces efficiently.

---

# C. MOEA/D

### Full Form

Multi-Objective Evolutionary Algorithm based on Decomposition

---

### Working

1. Decompose optimization problem.
2. Create scalar subproblems.
3. Solve simultaneously.
4. Exchange information among neighbors.

---

### Advantage

Strong exploitation capability.

Stable convergence.

---

# Greedy Baseline Method

Used for comparison.

Process:

1. Rank channels by accuracy.
2. Add channels one-by-one.
3. Stop at 16 channels.

Limitation:

No domain knowledge.

May select physiologically irrelevant channels.

---

# 4. FEATURE EXTRACTION

After selecting channels:

Authors use:

## FBCSP

Filter Bank Common Spatial Pattern

---

Frequency bands:

4–40 Hz

split into

9 non-overlapping bands.

---

Generated Features

### CSP Features

36 features per trial.

---

### Statistical Features

19 features per channel:

Examples:

- Mean
- Variance
- Standard deviation
- Entropy
- Skewness
- Kurtosis
- Hjorth parameters
- Zero-crossing rate

---

For 16 channels:

304 statistical features.

---

### Total Features

36 + 304

= 340 features

---

# Feature Selection

Authors use:

## mRMR

Minimum Redundancy Maximum Relevance

Purpose:

- Remove redundant features
- Keep informative features

Final retained features:

### Top 10 features

---

# Classification

Classifier:

## Support Vector Machine (SVM)

Hyperparameters optimized via Grid Search.

---

# 5. DATASETS

---

## Physionet

- 109 subjects
- 64 channels
- 160 Hz

---

## OpenBMI

- 54 subjects
- 62 channels
- 250 Hz

---

## HighGamma

- 14 subjects
- 128 channels
- 250 Hz

---

## BCIIV-2A

- 9 subjects
- 22 channels
- 250 Hz

---

# Preprocessing for All Datasets

Bandpass Filter:

4–40 Hz

to preserve:

- μ rhythm
- β rhythm

important for motor imagery.

---

# 6. RESULTS

## Classification Performance

### Physionet

| Method | Accuracy |
| --- | --- |
| NSGA-II | 83% |
| MOPSO | 87% |
| MOEA/D | 80% |
| Greedy | 93% |

MOPSO best among MOO methods.

---

### BCIIV-2A

| Method | Accuracy |
| --- | --- |
| NSGA-II | 61% |
| MOPSO | 63% |
| MOEA/D | 63% |
| Greedy | 65% |

---

### HighGamma

| Method | Accuracy |
| --- | --- |
| NSGA-II | 75% |
| MOPSO | 74% |
| MOEA/D | 69% |
| Greedy | 71% |

NSGA-II performed best.

---

### OpenBMI

| Method | Accuracy |
| --- | --- |
| NSGA-II | 67% |
| MOPSO | 71% |
| MOEA/D | 54% |
| Greedy | 69% |

MOPSO best performer.

---

# 7. Convergence Analysis

## NSGA-II

Converges:

200–500 generations.

Characteristics:

✔ Stable

✔ Good diversity

✔ Reliable Pareto front

---

## MOPSO

Converges fastest.

Characteristics:

✔ Fast exploration

✖ Sometimes oscillatory

✖ Sensitive to objective scaling

---

## MOEA/D

Characteristics:

✔ Stable

✔ Strong exploitation

✖ Longer optimization time

---

# 8. Selected Channels Analysis

The selected channels consistently cluster around:

### C3

### C4

### Cz

### FCz

### CPz

These regions correspond to:

### Sensorimotor Cortex

which is responsible for motor imagery processing.

---

# Key Observation

Even though algorithms differ:

All converge toward motor-cortex regions.

This validates the physiological relevance of the framework.

---

# 9. Statistical Analysis

ANOVA test performed.

Significant differences found for:

- Physionet
- OpenBMI
- BCIIV-2A

(p < 0.001)

---

HighGamma:

No significant difference.

All methods performed similarly.

---

# 10. Final Conclusion

## What did the authors achieve?

They developed a:

### Domain-Informed Multi-Objective Optimization Framework

for EEG channel selection.

---

## Core Innovation

Instead of optimizing:

```
Accuracy only
```

they optimize:

```
Spatial Relevance
+
Functional Discriminability
```

simultaneously.