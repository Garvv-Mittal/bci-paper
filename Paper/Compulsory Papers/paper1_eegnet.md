## EEGNet: A Compact Convolutional Neural Network for EEG-Based BCIs

---

## Paper Info

| Field | Details |
|-------|---------|
| **Title** | EEGNet: A Compact Convolutional Neural Network for EEG-Based Brain–Computer Interfaces |
| **Authors** | Vernon J. Lawhern, Amelia J. Solon, Nicholas R. Waytowich, Stephen M. Gordon, Chou P. Hung, Brent J. Lance |
| **Year** | 2018 (first appeared Nov 2016 on arXiv; published Jul 2018) |
| **Journal** | Journal of Neural Engineering (IOP Publishing) |
| **Volume/Article** | Vol. 15, No. 5, Article 056013 |
| **DOI** | [10.1088/1741-2552/aace8c](https://doi.org/10.1088/1741-2552/aace8c) |


Problem Addressed 
- problem 1: CNNs were paradigm specific
    - Traditionally, researchers design different processing methods for different EEG signals.
    - CNNs (they automate feature extraction from raw data) have been applied to single BCI paradigms 
    -  Most CNN studies tested it on only one task, not all of them together
- problem 2: interpretability
    - the learned features by the cnn are hard to interpret by humans 
    - in neuroscience, the ability to derive inputs is very important 
    - it is important that network performance is not being driven by noise or artifact signals 
    - authors want more than accuracy, they want to undersatnd what features the mechanism is using 
- problem 3: too many trainable parameters 
    - eeg datasets are small whereas CNNs have large parameters 
    - this causes the risk of overfitting 
- problem 4: existing CNNs did not include EEG specific features 
    - intitially CNNs were made for deep learning models and image recognition 
    - eeg specific features are usually not included
- problem 5: requires domain expertise 
    - feature extraction manually is time consuming 
    - need to be designed by experts individually 

Methodology
- Datasets used:
    1. P300 (rare and important stimuli)
    2. ERN (whenever error is generated)
    3. MRCP (movement related)
    4. SMR (rhythimic activity)
- EEGNet works by automatically learning which brain-wave frequencies are important, which electrodes carry those signals, and how to combine those patterns to classify a user's mental activity, all while using very few parameters so it can train on small EEG datasets.
- eeg data is divided into a 2D matrix where rows = electrodes and columns = time points



