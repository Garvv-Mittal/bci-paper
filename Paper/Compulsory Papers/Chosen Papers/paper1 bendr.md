## Title: BENDR: Using Transformers and a Contrastive Self-Supervised Learning Task to Learn From Massive Amounts of EEG Data

## Problem Statement 
Traditional EEG deep learning models suffer because:
- They require large labeled datasets.
- EEG labels are expensive.
- Models overfit individual subjects.
- Poor cross-subject and cross-dataset generalization.
- Different EEG recording setups make transfer difficult.
- accuracy drops between different subjects
- traditional supervised learning learns --> This person's EEG instead of --> What EEG generally looks like.
- large amounts of unlabeled data exists
## Methodology
- Self- Supervised Learning 
- Learns from unlabeled data by creating its own prediction task.
- Learn general EEG representations from massive unlabeled EEG using a BERT-style masked prediction task.
- they mask parts of EEG representations. The network predicts the missing information.
- This forces the model to understand EEG structure.
- Convolutional Encoder: Convert raw EEG into latent features.
- Masking: Random portions of latent representations are hidden.
- Transformer Context Network: Uses surrounding information to understand the missing regions.
## Key Results and Findings
- Self-supervised pretraining improves downstream EEG performance
- Strong performance with very little labeled data
- Learned representations are meaningful
## Limitations 
- Although BENDR learns transferable representations, it still requires fine-tuning, is not completely calibration-free, and does not fully solve cross-dataset variability. These limitations motivated later work such as EEGPT, which aims to learn more universal EEG representations across many datasets and tasks.
-