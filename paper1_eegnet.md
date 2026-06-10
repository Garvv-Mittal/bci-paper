EEGNet: A Compact Convolutional Neural Network for EEG-based Brain-Computer Interfaces

Abstract
problem statement: 
- different bci tasks (like response to stimuli, hand movement etc) produce different eeg signals
- researches have to design feature extraction methods manually for each type of movement 
- CNNs (perform automatic feature extraction) have only been tested on one task
- there is no generalized method
approach:
- EEGNet is a compact convolutional neural network for EEG-based BCIs.
- cross subject and within subject examiantion
- the paper tests eegnet on 4 different movements: P300, ERN, MRCP, SMR
results and significance 
- eegnet works wellon many different eeg tasks
- eeg sets are usually small and contain less data. eegnet is designed to use fewer parameters so it is suited well for eeg than cnns
- eegnet can handle both erp based and oscillatory based bci
- erp bci: short brain responses like triggers and response to stimuli, high in amplitude low in frequency, predictable, easy to learn
- oscillatory bci: motor imagery, asynchronous (not strictly tied to stimuli)

introduction
- different stages of bci processing are:
    1. data collection
    2. signal processing 
    3. feature extraction 
    4. classification stage 
    5. feedback stage 
- manual specification of stages 2,3 and 4 are required 
- CNNs use deep learning to automate the feature generation process
- eegnet uses two CNN convulations
    1. Depthwise convolutions
    2. Separable convolutions
- performs traditional eeg methods like filtering but with fewer trainable parameters
- eegnet makes cnns features more interpretable
- eegnet makes sure that the cnn is actually learning and not just removing noise 
- 