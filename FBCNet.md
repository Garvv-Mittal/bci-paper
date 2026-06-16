# FBCNet

OBJECTIVE

the objective of FBCNet is to improve motor imagery classification in EEG based Brain computer interface ( tries to understand these thoughts using signals recorded from the Brain through EEG electrode places at the scalp ) traditional EEG data suffers from problems like -

1. very weak and noisy signals. (the brain produces very small electrical signals these signals get mixed with eye blink, head movement, muscle activity etc)
2. high dimensional data 
3. limited training samples.

FBCNet ( filter bank convolutional network) introduced that instead of looking EEG signal as one single signal , divide it into multiple frequency band and learn from each band separately.

difficulties -

1. noisy , contain a large amount of data.
2. vary each time a person performs a task.
3. different imagined movement often produce similar brain signals.

Deep learning models should be evaluated on stroke patients because MI-BCI systems are useful for motor rehabilitation after a stroke, and traditional machine learning approaches may not perform effectively for all patients.———> deep learning may provide better rehabilitation outcomes.

METHODOLOGY 

The researchers proposed FBCNet (filter bank convolutional network) a deep learning model specially designed for Motor Imagery EEG classification.

dividing eeg into multiple frequency bands

Instead of treating EEG as one single signal, FBCNet first divides it into several frequency bands using a filter bank. Different motor imagery information may be present in different frequency ranges, so learning from each band separately helps the model capture more useful features.

learning spatial information

The filtered signals are then passed through a Spatial Convolution Block (SCB). This block learns which EEG channels and brain regions contain important information related to the imagined movement.

extracting temporal information

FBCNet uses a special Variance Layer to analyze changes in signal power over time. This is important because motor imagery tasks mainly differ in terms of EEG power changes known as ERD and ERS patterns. The Variance Layer also reduces the amount of data that needs to be processed.

classification

After extracting the important features, a fully connected layer and softmax classifier are used to classify the EEG signal into the correct motor imagery category.

training and evaluation

The model was trained using multiple EEG Motor Imagery datasets containing data from both healthy subjects and stroke patients. Its performance was compared with traditional machine learning methods and other deep learning models such as EEGNet and Deep ConvNet

understanding model decisions

The researchers also used DeepLift analysis to check whether FBCNet was learning meaningful brain activity patterns instead of noise. This helped explain how the model made its decisions and highlighted differences between healthy individuals and stroke patients.

RESULT 

The results showed that FBCNet performed better than traditional machine learning methods and other deep learning models on all the tested datasets. It achieved 76.20% accuracy on the BCIC-IV-2A dataset, which was the best reported result for this dataset.

The model also showed better performance when only a small amount of training data was available. While other deep learning models experienced a noticeable drop in accuracy, FBCNet was still able to learn useful patterns from the EEG signals.

Another important finding was that the Variance Layer worked better than commonly used Average Pooling and Max Pooling layers for extracting temporal information from EEG data.

The interpretability analysis showed that FBCNet was focusing on important motor-related brain regions such as C3 and C4, which are known to be associated with motor imagery tasks. This suggests that the model was learning meaningful brain activity patterns rather than noise.

For stroke patients, the important EEG patterns were more different from one person to another and were spread across a larger number of brain regions. This indicates that after a stroke, the brain may use additional areas to compensate for damaged motor regions.