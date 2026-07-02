**NEED: Cross-Subject and Cross-Task Generalization for Video and Image Reconstruction from EEG Signals**

OBJECTIVE-

This paper aimed to solve two major problems in EEG-based visual reconstruction systems.

What usually happens is that EEG models can reconstruct images or videos only for the subjects they were trained on. When a new user comes in, the performance often drops because EEG signals vary from person to person.

Another limitation is that most existing models are task-specific. A model trained for image reconstruction usually cannot be used for video reconstruction and vice versa.

So the aim was to develop a single EEG decoding framework that could work across different subjects as well as different reconstruction tasks.

The researchers proposed a framework called NEED (Neural Decoding with Enhanced Extensibility and Diversity) to achieve this.

METHODOLOGY-

1. The researchers developed a unified framework called NEED.
    
    The main idea was to create a system that could reconstruct both images and videos from EEG signals while also working for unseen subjects.
    
2. The framework consists of three main components:Individual Adaptation Module (IAM),Dual-Stream EEG Encoder (DSGNet),Unified Inference Mechanism
3. One of the biggest challenges in EEG decoding is that brain signals are different for every individual.To solve this problem, the researchers proposed IAM.
    
    IAM reduces subject-specific differences and converts EEG signals into a common feature space so that the model can generalize better to new users.
    
4. The researchers also developed a new EEG encoder called DSGNet.
    
    It contains two parallel streams.
    
    The spatial stream learns relationships between different brain regions, while the temporal stream learns how EEG signals change over time.
    
    Both streams are combined using an attention mechanism to create richer EEG representations.
    
5. The framework extracts two types of information from EEG signals.
    
    Perception Understanding (PU), which focuses on visual details such as motion and scene structure.
    
    Semantic Understanding (SU), which focuses on objects, concepts and scene meaning.
    
6. Instead of creating separate systems for images and videos, NEED uses a unified inference mechanism.
    
    The model automatically adapts itself depending on whether image reconstruction or video reconstruction is required.
    
7. The framework was trained and tested using multiple EEG datasets to evaluate both cross-subject and cross-task performance.

RESULTS-

1. The researchers found that NEED was able to generalize well across different subjects.
    
    Unlike traditional models that perform well only on trained subjects, NEED maintained good performance even on unseen users.
    
2. Another important finding was that the framework could work across different reconstruction tasks.
    
    A model trained for video reconstruction was also able to reconstruct images without additional training, showing strong cross-task generalization.
    
3. The proposed DSGNet encoder learned richer EEG features and performed better than several existing EEG decoding approaches.
4. The researchers also found that IAM was one of the most important components of the framework because it helped reduce subject-specific variations and improve overall performance.
5. Overall, NEED successfully reconstructed both images and videos from EEG signals while maintaining good performance across different users and tasks.

CONCLUSION-

This paper proposes NEED (Neural Decoding with Enhanced Extensibility and Diversity), a unified EEG decoding framework for visual reconstruction.

What usually happens is that EEG reconstruction models work only for the subjects they were trained on and are usually designed for a single task.

NEED addresses both of these limitations by introducing subject adaptation, dual-stream EEG feature extraction and a unified reconstruction mechanism.

The framework can reconstruct both images and videos from EEG signals, generalize to new users and transfer across different reconstruction tasks without retraining.

Overall, this study is an important step towards building more practical and generalizable EEG-based Brain-Computer Interface systems for visual decoding.
