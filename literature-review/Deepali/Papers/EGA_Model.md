# EGA  Model

# *Entropy Guided Adversarial Model for Weakly Supervised Object Localization (EGA Model)*

Authors: Sabrina Narimene Benassou, Wuzhen Shi, Feng Jiang

---

# 1. Abstract

### Problem

Weakly Supervised Object Localization (WSOL) aims to locate objects in images using only image-level labels, without bounding-box annotations.

Traditional CAM-based methods highlight only the most discriminative parts of objects (e.g., bird head instead of whole bird).

### Existing Solutions

1. **Hide object parts**
    - Remove discriminative regions.
    - Force CNN to find other features.
    - Causes information loss.
2. **Modify network architecture**
    - Generate multiple CAMs.
    - Increases complexity.

### Proposed Solution

The paper introduces:

## EGA (Entropy Guided Adversarial Model)

Combines:

- Adversarial Learning
- Shannon Entropy Minimization

Advantages:

- No image erasing.
- No network modification.
- Better localization and classification.

Main idea:

Training on adversarial examples forces CNNs to discover more object features.

Entropy minimization further expands object activation regions.

---

# 2. Introduction

## What is WSOL?

Weakly Supervised Object Localization:

Input:

- Image
- Class label

Output:

- Object location

without using bounding boxes.

---

## Role of CAM

Class Activation Map (CAM):

Shows image regions responsible for classification.

Problem:

CAM only focuses on the most discriminative feature.

Example:

Bird → only head activated.

Entire body remains ignored.

---

## Existing Approaches

### Approach 1: Hide Regions

Methods:

- ACoL
- HaS
- ADL
- CutMix

Process:

1. Find discriminative region.
2. Remove it.
3. Force network to learn remaining object parts.

Problems:

- Information loss.
- Learns background features.
- Reduces recognition ability.

---

### Approach 2: Multi-Level CAM

Methods:

- SPG
- DANet
- CCAM

Process:

Generate CAMs from multiple network levels.

Problems:

- Network architecture must be modified.
- Hard to generalize.

---

## Authors' Observation

Adversarially trained networks learn:

- clearer edges
- borders
- meaningful structures

instead of noisy patterns.

This observation inspired EGA.

---

# Key Contributions

### Contribution 1

Use adversarial examples to activate more object features.

### Contribution 2

Introduce entropy loss on CAM.

### Contribution 3

Achieve state-of-the-art WSOL performance.

---

# 3. Related Work

---

## 3.1 Weakly Supervised Object Localization

### CAM (Zhou et al., 2016)

Introduced Global Average Pooling (GAP).

Generated localization maps using class labels only.

Problem:

Only discriminative regions are highlighted.

---

### ACoL

Two-branch architecture.

Branch 1:

Finds most discriminative region.

Branch 2:

Learns remaining object parts.

---

### Hide-and-Seek (HaS)

Image divided into patches.

Random patches hidden during training.

CNN learns whole object.

---

### ADL

Uses attention mechanism and dropout masks.

---

### CutMix

Removes discriminative regions and replaces them with patches from other images.

---

### SPG

Uses high-level attention maps to guide low-level maps.

---

### DANet

Uses hierarchical labels.

Learns shared visual patterns.

---

### CCAM

Combines multiple CAMs generated from different layers.

---

# 3.2 Adversarial Learning

## Definition

Training with intentionally perturbed images.

Goal:

Improve robustness.

---

## Important Works

### Madry et al.

Robust optimization framework.

### Goodfellow et al.

FGSM adversarial training.

Recommended mixing:

- Clean samples
- Adversarial samples

for regularization.

### Tsipras et al.

Showed adversarially trained networks learn better features.

---

## Why Useful for WSOL?

Adversarial images force CNN to:

Look beyond most discriminative region.

Result:

More complete object localization.

---

# 3.3 Entropy

## Shannon Entropy

Measures uncertainty.

Low entropy:

High confidence.

High entropy:

Low confidence.

---

### Interpretation for CAM

Activated pixels:

- High confidence
- Low entropy

Non-activated object pixels:

- Low confidence
- High entropy

Thus:

Reducing entropy can encourage activation of missing object regions.

---

# 4. Proposed Method (EGA)

The complete framework is shown in Figure 2 of the paper.

---

# 4.1 Revisiting CAM

Let:

fk(h,w)f_k(h,w)fk(h,w)

be feature maps from the last convolution layer.

### Global Average Pooling

Fk=∑(h,w)fk(h,w)F_k=\sum_{(h,w)}f_k(h,w)Fk=∑(h,w)fk(h,w)

Produces feature vector.

---

### Class Score

Sc=∑kakc∑(h,w)fk(h,w)S_c=\sum_k a_k^c \sum_{(h,w)} f_k(h,w)

Sc=k∑akc(h,w)∑fk(h,w)

where:

akca_k^cakc

= importance of feature map k for class c.

---

### CAM Generation

CAMc(h,w)=∑kakcfk(h,w)CAM_c(h,w)=\sum_k a_k^c f_k(h,w)

CAMc(h,w)=k∑akcfk(h,w)

Highlights discriminative regions.

---

# 4.2 Adversarial Learning in EGA

Traditional training:

min⁡θL(θ,x,y)\min_\theta L(\theta,x,y)

θminL(θ,x,y)

---

## Robust Training

min⁡θmax⁡ϵ∈δL(θ,x+ϵ,y)\min_\theta \max_{\epsilon\in\delta}L(\theta,x+\epsilon,y)

θminϵ∈δmaxL(θ,x+ϵ,y)

where:

ε = perturbation.

---

## Mixed Training

Paper uses:

Lclean+LadvL_{clean}+L_{adv}

Lclean+Ladv

as suggested by Goodfellow.

---

## Auxiliary Batch Normalization

Problem:

Clean and adversarial images follow different distributions.

Solution:

Two BatchNorm layers:

### Main BN

Used for clean images.

### Auxiliary BN

Used for adversarial images.

During testing:

Only main BN is retained.

---

# EGA Workflow

Step 1:

Input clean image.

Step 2:

Generate adversarial image.

Step 3:

Feed both through network.

Step 4:

Generate

- CAMclean
- CAMadv

Step 5:

Apply entropy loss.

Step 6:

Update network.

---

# 4.3 Entropy Minimization

Authors generate CAMs during training itself.

---

## Entropy Loss

Lent(CAM)=−∑(h,w)PCAM(h,w)log⁡PCAM(h,w)L_{ent}(CAM)
=
-\sum_{(h,w)}
P_{CAM(h,w)}
\log
P_{CAM(h,w)}

Lent(CAM)=−(h,w)∑PCAM(h,w)logPCAM(h,w)

Lent(CAM)=−∑(h,w)PCAM(h,w)log⁡PCAM(h,w)L_{ent}(CAM)=-\sum_{(h,w)}P_{CAM(h,w)}\log P_{CAM(h,w)}Lent(CAM)=−∑(h,w)PCAM(h,w)logPCAM(h,w)

Meaning:

Pixels with low confidence receive higher penalty.

Result:

CNN expands object activation regions.

---

# 4.4 Final Loss Function

Combines:

1. Classification Loss
2. Adversarial Loss
3. Entropy Loss

Ltotal=Lclean+Ladv+λcleanLent(CAMclean)+λadvLent(CAMadv)L_{total}
=
L_{clean}
+
L_{adv}
+
\lambda_{clean}L_{ent}(CAM_{clean})
+
\lambda_{adv}L_{ent}(CAM_{adv})

Ltotal=Lclean+Ladv+λcleanLent(CAMclean)+λadvLent(CAMadv)

Key Idea:

λclean>λadv\lambda_{clean}
>
\lambda_{adv}

λclean>λadv

Because CAMclean activates fewer pixels.

---

# 5. Experimental Setup

## Datasets

### CUB-200-2011

- 200 bird classes
- 11,788 images

Training:

- 5,994

Testing:

- 5,794

---

### ILSVRC (ImageNet)

- 1000 classes
- 1.2 million training images

---

### OpenImages

- 100 classes
- 29,819 training images

---

# Evaluation Metrics

## Classification

Top-1 Accuracy

---

## Localization

### Top-1 Localization Accuracy

Correct class + IOU > 0.5

---

### CorLoc

Correct localization regardless of classification.

---

### MaxBoxAccV2

Average IOU over:

- 0.3
- 0.5
- 0.7

thresholds.

---

### PxAP

Used for OpenImages.

Measures pixel precision-recall.

---

# Network Architectures

Backbones:

### VGG-16

### GoogLeNet

Images resized:

256×256

Random crop:

224×224

Pretrained on ImageNet.

---

# 6. Results

---

# CUB Dataset

### VGG-EGA

Classification Error:

21.87%

Localization Error:

40.84%

Best overall performance.

---

Improvement over CAM:

Classification:

1.53%

Localization:

15.01%

---

# ILSVRC Dataset

### GoogLeNet-EGA

Classification Error:

27.42%

Localization Error:

50.17%

Outperformed previous methods.

---

### VGG-EGA

Classification Error:

29.36%

Localization Error:

52.69%

Very competitive results.

---

# OpenImages Dataset

### VGG-EGA

Classification Error:

30.0%

Localization Error:

38.21%

New state-of-the-art localization result.

---

# Visual Results (Figures 5 & 6)

The paper shows that EGA:

- Activates larger object regions.
- Produces tighter bounding boxes.
- Reduces background activation.

Compared with CAM.

---

# 7. Ablation Study

---

## Effect of Adversarial Learning

### VGG

CAM:

55.85 localization error

↓

Adversarial Learning:

40.93

Huge improvement.

---

## Effect of Entropy

Further improves:

40.93 → 40.84

Localization becomes slightly better.

---

# Perturbation Strength Study

PGD perturbation:

ε = 1,2,3,4

Observation:

Larger perturbation →

Higher error.

Best result:

ε = 1

Small perturbations are sufficient.

---

# Regularization Factor Study

Best parameters:

λclean=1\lambda_{clean}=1

λclean=1

λadv=0.01\lambda_{adv}=0.01

λadv=0.01

These produced the best localization accuracy.

---

# Final Conclusion (Exam/Interview Ready)

### Core Idea

Instead of removing object parts or modifying the CNN architecture:

The paper trains the model using:

1. Clean images
2. Adversarial images
3. Entropy minimization

to force the network to discover more object regions.

---

### Why EGA Works

Adversarial training:

 Learns richer object features

 Acts as data augmentation

 Improves generalization

Entropy minimization:

 Reduces uncertainty

 Activates missing object pixels

 Expands CAM coverage