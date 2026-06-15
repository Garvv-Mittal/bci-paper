# EEGNet Research Notes

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

### Block 1: Temporal + Spatial Feature Extraction

#### Step 1: Temporal Convolution (1×64)
- Operates only along time.
- Kernel length = half sampling frequency.
- Learns frequency-selective filters automatically.

#### Step 2: Depthwise Spatial Convolution (C×1)
- Learns spatial filters across electrodes.
- Functions as a trainable FBCSP replacement.
- Depth multiplier D controls filters per temporal map.

### Block 2: Feature Summarization

#### Step 3: Depthwise Separable Convolution
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
