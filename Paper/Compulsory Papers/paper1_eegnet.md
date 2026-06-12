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


## Problem Addressed 
- **problem 1**: CNNs were paradigm specific
    - Traditionally, researchers design different processing methods for different EEG signals.
    - CNNs (they automate feature extraction from raw data) have been applied to single BCI paradigms 
    -  Most CNN studies tested it on only one task, not all of them together
- **problem 2**: interpretability
    - the learned features by the cnn are hard to interpret by humans 
    - in neuroscience, the ability to derive inputs is very important 
    - it is important that network performance is not being driven by noise or artifact signals 
    - authors want more than accuracy, they want to undersatnd what features the mechanism is using 
- **problem 3**: too many trainable parameters 
    - eeg datasets are small whereas CNNs have large parameters 
    - this causes the risk of overfitting 
- **problem 4**: existing CNNs did not include EEG specific features 
    - intitially CNNs were made for deep learning models and image recognition 
    - eeg specific features are usually not included
- **problem 5**: requires domain expertise 
    - feature extraction manually is time consuming 
    - need to be designed by experts individually 

## Methodology
- **Datasets used:**
    1. P300 (rare and important stimuli)
    2. ERN (whenever error is generated)
    3. MRCP (movement related)
    4. SMR (rhythimic activity)
- **What is EEGNET?**
    - EEGNet works by automatically learning which brain-wave frequencies are important, which electrodes carry those signals, and how to combine those patterns to classify a user's mental activity, all while using very few parameters so it can train on small EEG datasets.
    - eeg data is divided into a 2D matrix where rows = electrodes and columns = time points

- Layer 1: Temporal Convulation
    - applied along time
    - it replaces the traditional method of learning frequency filters 
    -  the first convolutional layer learns useful frequency patterns directly from data.
- Layer 2: Depthwise Convolution
    - Each temporal filter gets its own spatial filter.
    - Depthwise Conv ≈ traditional Spatial Filtering
    - learns which electrodes are useful
- Layer 3 : Separable Convolution
    - combines depthwsie convolution + temporal convolution
    - the network learns relationship among features 

- **Comparison between EEGNET and tradiitonal methods**
| Methodology Component | Purpose |
| Temporal Convolution	Learn |  frequency-specific filters |
| Depthwise Convolution	Learn | spatial filters across electrodes |
| Batch Normalization |	 Stabilize training |
| ELU Activation | Non-linear feature extraction |
| Average Pooling |	 Dimensionality reduction |
| Dropout | Prevent overfitting |
| Separable Convolution	Combine |  learned EEG features efficiently |
| Softmax Layer| Final classification |

## Key Results and Findings 
- **Comparison with other CNN models**
    - The authors compare EEGNet mainly against: DeepConvNet (a deep CNN for EEG) and ShallowConvNet (a shallower CNN designed for band-power features)
    - against deepconvet, it showed similar results but with much fewer trainable parameters
    - shallownet performed well mostly in motor imagery because that is what it was designed for, however eegnet maintained accuracy throughout the 4 paradgims used 
    - eegnet's main advantge is parameter efficiency
- ** Comparison with Traditional Approaches**
    - The paper compares against xDAWN + classifier approaches. xDAWN is a spatial filtering technique specifically designed to enhance P300 responses.
    - for SRM and motor imagery, the traditional method was CSP + LDA
    - The paper shows that EEGNet achieves performance comparable to these carefully engineered pipelines.
    - This is significant because traditional methods were developed through years of EEG research and domain expertise.
    - EEGNET is also a single architecture for different paradigms 
- ** Cross Subject Transfer learning **
    - performance dropped but EEGNet still performed similarly to or better than larger CNNs.
    - The EEGNet results suggested that compact EEG-specific architectures could learn more robust representations.
- ** Other results**
    - EEGNet demonstrated that its learned representations correspond to established neuroscience.
    - this increases trust in the model 
    - doesnt behave like a black box unlike other deep learning models 

##Limitations 
- as the goal was to achieve a compact architectutre, so accuracy was not always maximum 
- performance of cross subujected training dropped when compared to within subject training 
- Practical online BCI performance was not thoroughly demonstrated.
- it still needs some tuning and expert guidance 

## Future Scope
- testing on more paradigms than just 4 
- improving cross subject accuracy 
- adapting to larger data sets
- eegnet is a useful starting point

## Relevance to our project
- this paper discusses how we can use one compact ssytem for different bci paradigms, which aligns with the concept of out project of making a generalizabke deep learning framework 
- it tests cross subject accuracy which is useful for our project







