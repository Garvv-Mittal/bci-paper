## Problem Statement 
Traditional EEG deep learning models suffer because:
- They require large labeled datasets.
- EEG labels are expensive.
- Models overfit individual subjects.
- Poor cross-subject and cross-dataset generalization.
- Different EEG recording setups make transfer difficult.
## Methodology
- Self- Supervised Learning 
- Learns from unlabeled data by creating its own prediction task.
- Learn general EEG representations from massive unlabeled EEG using a BERT-style masked prediction task.
- Convolutional Encoder: Convert raw EEG into latent features.
- Masking: Random portions of latent representations are hidden.
- Transformer Context Network: Uses surrounding information to understand the missing regions.
- 