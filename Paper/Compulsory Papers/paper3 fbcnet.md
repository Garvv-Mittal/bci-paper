## FBCNet: A Multi-view Convolutional Neural Network for Brain-Computer Interface

---

## Paper Info

| Field | Details |
|-------|---------|
| **Title** |FBCNet: A Multi-view Convolutional Neural Network for Brain-Computer Interface
| **Authors** |Ravikiran Mane Member, IEEE, Effie Chew, Karen Chua, Kai Keng Ang Senior Member, IEEE, Neethu Robinson Member, IEEE, A. P. Vinod Senior Member, IEEE, Seong-Whan Lee Fellow, IEEE, and Cuntai Guan Fellow, IEEE|

## Problems Addressed 

- eeg siganls contain different frequency bands
- cnn models like deepconvnet recieve these mixed signals 
- there is a need for a model that can learn from multiple frequency bands separately 
- frequency specific filtration is not being adopted 

They propose FBCNet to:
- Extract spatial features separately from multiple frequency bands.
- Learn frequency-specific representations.
- Improve cross-subject MI classification performance.

## Methodology
- FBCNet is designed specifically for Motor Imagery EEG Classification
Step 1: Multi-view data representation:
    - EEG signals contain information at different frequencies.
    - Motor imagery information is mainly present in the Mu and Beta bands.
    - The authors therefore separate the EEG into multiple frequency bands before giving it to the CNN.
    - The EEG is passed through 9 bandpass filters, each filter extracts only its own frequency range.
    - The paper calls this a multi-view representation.
Step 2: Spatial Localization (Spatial Convolution Block)
    - authors use depthwise  convolution to find which electrodes are useful
    - Depthwise convolution processes each frequency band independently.
    - each frequency band gets its own spatial filter 
    - kernel size = (C,1)
    - after convolution, batch normalization takes place
    - Without an activation function, every layer would perform only linear operation
    - No matter how many layers you stack, the network would behave like a single linear model.
    - here we use swish activation
    - Weight Normalization: ∣∣w∣∣<2 reduces overfitting 
Step 3: Temporal Feature Extraction (Variance Layer)
    - The time dimension is huge. Many of these values are: noisy, redundant, prone to overfitting
    -  to combat this, traditional methods use pooling 
    - but in motor imagery power is more informative
    - The Variance Layer calculates this for every feature map.
    - Backpropagation Analysis: Points far from the mean receive larger gradients.
    - Motor imagery produces significant deviations: ERD and ERS
    - These deviations carry class information.
    - Variance Layer naturally focuses learning on those deviations.
    - Window-Based Variance: Instead of computing one variance over the entire signal, they use windows of different time periods
    - Before variance: m × Nb × T and After variance: m × Nb × (T/w) --> Huge reduction in features.
Step 4: Classification
    - Log Activation : This compresses large power values.
    - Fully Connected Layer: Receives all extracted features and learns
    - Softmax: Converts outputs into probabilities

Complete Flow:
Raw EEG
(C × T)
    ↓
Filter Bank
(9 frequency bands)
    ↓
Multi-view EEG
(9 × C × T)
    ↓
Depthwise Spatial Convolution
(learn spatial filters)
    ↓
BatchNorm + Swish
    ↓
Spectro-Spatial Features
    ↓
Variance Layer
(extract EEG power)
    ↓
Feature Reduction
    ↓
Log Activation
    ↓
Fully Connected Layer
    ↓
Softmax
    ↓
Motor Imagery Class
 
## Results and Findings 
- The authors compared FBCNet against: FBCSP, ShallowConvNet, DeepConvNet, EEGNet, Other CNN-based architectures: it outperformed them all
- This demonstrates that combining: Filter-bank representation, Spatial CNN filtering, Variance Layer is more effective than existing architectures.
- FBCNet consistently outperformed EEGNet
- FBCNet performed better than competing methods in cross-session evaluation.
- Using multiple frequency bands significantly improved classification performance.
- The proposed Variance Layer produced better results than pooling
- FBCNet achieved strong performance despite EEG datasets being relatively small.
- Optimal Number of Spatial Filters was found to be 32

## Relevance to project
- spatial filtering, variance layer and filter bank representation lead to increased accuracy in cross session experimentation