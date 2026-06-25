# Computer - Aided Tumor Diagnosis In Automated Breast Ultrasound sing 3D Detection Network

## Research Domain ( Abstarct+Intro)

- AI in Healthcare
- Medical Imaging
- Deep Learning
- Computer Vision
- Breast Cancer Detection

### Objectives

The paper proposes a **3D deep learning detection network** for:

1. Detecting lesions/tumors in Automated Breast Ultrasound (ABUS) images
2. Classifying tumors as:
- Benign
- Malignant

The goal is to reduce:

- manual diagnosis effort
- false detections
- diagnosis time

while improving:

- detection accuracy
- sensitivity
- automated diagnosis capability

## Background Concepts

### What is Breast Cancer?

Breast cancer occurs when abnormal cells grow uncontrollably in breast tissue.

Early detection:

- improves survival rate
- reduces mortality
- allows faster treatment

### What is Ultrasound Imaging?

Ultrasound imaging uses:

- high-frequency sound waves
- non-invasive scanning

to generate internal body images.

Advantages:

- safe
- radiation-free
- affordable
- suitable for dense breast tissue

## What is ABUS?

### ABUS Meaning

ABUS = **Automated Breast Ultrasound** It is:

- a modern breast imaging technique
- capable of generating 3D breast images
- useful in breast cancer diagnosis

---

### Advantages of ABUS

According to the paper:

ABUS provides:

- coronal plane visualization
- improved diagnostic value compared to traditional ultrasound.

## Problems Mentioned in the Paper

Even though ABUS is powerful, the paper states several problems:
****

### Challenges

#### 1. Manual screening is time-consuming

Doctors must inspect huge image volumes.

---

#### 2. Tumors may be overlooked

Radiologists can miss abnormalities during screening.

---

#### 3. High workload

ABUS scans generate many image slices.

---

#### 4. Poor image quality

Compared to other imaging methods:

- ultrasound quality is lower
- lesion boundaries are difficult to identify

---

#### 5. Small lesion area

The lesion region is often:

- less than 1% of the total image making detection difficult.

---

#### 6. Benign and malignant tumors appear similar

This creates:

- classification difficulty
- higher false detections

---

#### 7. Huge computational demand

A reconstructed ABUS image:

- contains approximately 800 frames which requires: high computing resources.

## Proposed Solution

Two-Stage 3D Detection Framework

---

### Stage 1

#### Lesion Localization

The network identifies suspicious tumor regions.

---

### Stage 2

#### Tumor Classification

The detected lesion is classified as:

- benign
- malignant

## Important Technical Concepts Mentioned

### 3D Detection Network

The paper uses:

- 3D convolutional neural networks
- volumetric learning

to better understand:

- spatial tissue information.

---

### Similarity Loss

A novel similarity loss is introduced to:

- distinguish lesions from background tissues
- improve lesion detection quality.

---

### Joint Learning Strategy

The paper balances:

- classification task
- localization task to improve overall model performance.

---

## Dataset Information

The dataset contains:

- 418 patients
- 145 benign tumors
- 273 malignant tumors

### **Performance Results Mentioned**
Sensitivity

97.66%

Meaning:

- the system successfully detects most tumors.

---

### False Positives

12.3 FPs

Meaning:

- some normal tissues are still incorrectly detected as tumors.

---

### AUC Score

0.8720

This indicates:

- strong classification performance.

## Introduction

#### Breast Cancer Importance

The paper states:Breast cancer is one of the most commonly diagnosed cancers worldwide

Early screening:

-significantly reduces mortality rates.

### Comparison Between HHUS and ABUS

#### HHUS

Hand-Held Ultrasound

Problems:

- operator dependent
- low repeatability

---

#### ABUS

Automated Breast Ultrasound

Advantages:

- automated scanning
- better reproducibility
- 3D information
- coronal plane visualization

## 

# Need for Computer-Aided Diagnosis (CAD)

The paper highlights that:

manual diagnosis alone is insufficient because:

- doctors can miss lesions
- workload is very high
- screening takes time

Therefore: CAD systems are necessary.

## Previous Research Mentioned

The paper references earlier CAD systems using:

- ensemble neural networks
- CNNs
- quantitative tissue clustering
- 3D CNN models
- focal loss
- ensemble learning

---

## 15. Limitation of Previous Systems

Existing systems still suffer from:

- lower sensitivity
- false positives
- weak lesion localization
- poor classification performance

## Key Idea of This Paper

The core innovation is:

> Combining 3D lesion localization + tumor classification + similarity loss into one efficient framework.
> 

## workflow

ABUS Image
↓
3D Detection Network
↓
Lesion Localization
↓
Feature Extraction
↓
Tumor Classification
↓
Benign / Malignant Output

[Proposed Network Architecture ](https://www.notion.so/Proposed-Network-Architecture-35e59b545296805f8cccc0e1ae48cc66?pvs=21)

[](https://www.notion.so/35e59b54529680b29613ff3f828eaef9?pvs=21)