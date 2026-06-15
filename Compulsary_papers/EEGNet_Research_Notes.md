# EEGNet Research Notes
## Paper Info

| Field | Details |
|-------|---------|
| **Title** | EEGNet: A Compact Convolutional Neural Network for EEG-Based Brain–Computer Interfaces |
| **Authors** | Vernon J. Lawhern, Amelia J. Solon, Nicholas R. Waytowich, Stephen M. Gordon, Chou P. Hung, Brent J. Lance |
| **Year** | 2018 (first appeared Nov 2016 on arXiv; published Jul 2018) |
| **Journal** | Journal of Neural Engineering (IOP Publishing) |
| **Volume/Article** | Vol. 15, No. 5, Article 056013 |
| **DOI** | [10.1088/1741-2552/aace8c](https://doi.org/10.1088/1741-2552/aace8c) |

## Core Idea
Developed by the U.S. Army Research Laboratory to create a single ultra-compact CNN (~2,000 parameters) capable of classifying raw EEG signals across multiple BCI paradigms without extensive handcrafted feature engineering.

## Problems Solved
### 1. One-Trick Pony Architectures
- Traditional EEG networks were task-specific.
- Models designed for P300 often failed on Motor Imagery tasks.
- Required paradigm-specific redesigns.

### 2. Small Data vs Big Model Paradox
- EEG datasets are small, noisy, and expensive.
- Large CNNs overfit easily.
- EEGNet uses ~2,000 parameters.

### 3. The Black Box Problem
- Researchers need neurophysiological interpretability.
- EEGNet layers map directly to known EEG processing concepts.

### 4. Human Feature Engineering Bottleneck
- Replaces CSP, FBCSP, and xDAWN style handcrafted pipelines.
- Learns temporal and spatial filters directly from raw EEG.

## Architecture
# Block 1: Slicing Through Time & Space

### Step 1: Temporal Conv (1 x 64 kernel)
* **What it does**: Slides exclusively along the time axis. 
* **The Logic**: The kernel length is intentionally locked to exactly half of the data's sampling rate (for 128 Hz data, kernel size is 64). This acts as a data-driven frequency sweep, natively capturing clean brain wave oscillations from 2 Hz and up (covering delta, theta, alpha, and beta bands) without manual bandpass filtering.
- Operates only along time.
- Kernel length = half sampling frequency.
- Learns frequency-selective filters automatically.
- 
### Step 2: Depthwise Conv (C x 1 kernel)
* **What it does**: Applies a standalone spatial filter to each temporal feature map individually, completely avoiding cross-channel connections. 
* **The Logic**: This mathematically replaces the classic Filter-Bank Common Spatial Pattern (FBCSP) pipeline—except it learns the spatial weights dynamically from raw data *(sab kuch auto-tune hota hai)*. It isolates exactly which scalp electrodes matter most for a specific frequency band. A depth multiplier (D) controls the number of spatial filters learned per map.
- Learns spatial filters across electrodes.
- Functions as a trainable FBCSP replacement.
- Depth multiplier D controls filters per temporal map.


---

# Block 2: The Data Summarizer

### Step 3: Depthwise Separable Conv (1 x 16 kernel)
* **What it does**: First, a light depthwise layer summarizes an individual feature map over a short time window (500 ms at 32 Hz pooling). Then, a 1 x 1 pointwise convolution steps in to optimally blend the resulting maps together. 
* **The Logic**: This decouples spatial profiling from temporal mixing, drastically slashing the parameter count.
- Depthwise convolution summarizes individual feature maps.
- Pointwise (1×1) convolution combines feature maps.
- Greatly reduces parameter count.

## Important Training Decisions

### Linear Convolutions
Temporal and spatial convolutions remain linear.

### ELU Activation
- Used after BatchNorm.
- Prevents dead neurons.
- More stable on noisy EEG data.

### Average Pooling
- Captures sustained oscillatory power.
- Less sensitive to artifacts than max pooling.

### Dynamic Dropout
- Within-subject: 0.5
- Cross-subject: 0.25

### Max-Norm Constraint
- Spatial weights constrained.
- Prevents exploding weights.

## Validation

Tested on:
- P300
- ERN
- MRCP
- SMR

### Parameter Efficiency
| Model | Parameters |
|---------|---------|
| EEGNet | ~2K |
| ShallowConvNet | ~40K |
| DeepConvNet | ~150K |

### Interpretability
Verified using:
- DeepLIFT
- Weight correlations
- Ablation studies

## Limitations

### Cross-Subject Performance
Still struggles with biological variability between subjects.

### No Domain Adaptation
Cannot handle:
- Fatigue
- Mood changes
- Electrode shifts
- Session drift

### Manual Hyperparameter Tuning
Requires adjustment of:
- Kernel size
- Dropout
- Depth multiplier

### Limited Long-Term Modeling
Convolution windows only capture local temporal patterns.

## Connection to Our Project

### Backbone
Use EEGNet as the primary feature encoder.

### Baseline
Compare all experiments against standard EEGNet.

### Opportunity
EEGNet does not solve:
- Cross-subject variability
- Cross-session drift

### Proposed Extension
EEGNet + Domain Adaptation

Possible methods:
- DANN
- CORAL Loss
- MMD Loss

## Final Takeaway
EEGNet proves that carefully designed neurophysiological inductive biases can outperform much larger CNNs on EEG data while using only a fraction of the parameters.

Most promising direction:
**EEGNet + Domain Adaptation**

## Our Exact Subsystem: 
We will take this ultra-compact EEGNet encoder backbone and layer domain adaptation or adversarial transfer learning techniques directly on top of it. This allows us to bridge the biological and temporal cross-subject gaps that this classic paper leaves completely wide open.
