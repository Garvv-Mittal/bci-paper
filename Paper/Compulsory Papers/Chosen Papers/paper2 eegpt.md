## Title: EEGPT: Unlocking the Potential of EEG Generalist Foundation Models

## Problems Addressed 
- Existing self-supervised methods are still limited
- While BENDR demonstrated that self-supervised learning is effective, it still has limitations:
    - relatively limited pretraining data,
    - fewer downstream tasks,
    - no truly general EEG model,
    - limited cross-dataset capability.
    - Like BENDR, EEGPT learns from unlabeled EEG. However, the scale of pretraining is much larger.

## Methodology
- Instead of pretraining on one dataset, why not pretrain on many EEG datasets simultaneously?
- A foundation model is a model that is pretrained on a very large and diverse collection of data so that it learns general representations which can later be adapted to many downstream tasks
- The authors preprocess and standardize the EEG recordings so that they have a consistent format before being fed into the model.
- Patch Embedding (Tokenization): Instead of processing every EEG sample individually, EEGPT divides the EEG signal into small temporal patches. This is similar to words in BERT
- Self-Supervised Pretraining
- Transformer Encoder: The masked EEG tokens are passed through a Transformer.
- Traditional CNNs mainly capture local patterns. Transformers use self-attention, allowing them to model relationships between distant parts of the EEG sequence.
- EEGPT is built as a foundation model, not a task-specific classifier.

## Key Results and Findings 
- model learned general EEG knowledge
- addressed the issue of Poor cross-dataset generalization
- Better Cross-Subject Performance
- Better Performance with Limited Labeled Data
- The paper shows that increasing the scale and diversity of pretraining data leads to better downstream performance.

## Limitations 
- Not Completely Calibration-Free, it still requires fine-tuning.
- Foundation models are expensive to train.
- Many research groups or hospitals do not have the computational resources needed to reproduce or extend EEGPT.
- The success of EEGPT relies heavily on having access to large and diverse EEG datasets. If only a small dataset is available, it is difficult to train a comparable foundation model from scratch.
- Limited robustness to real-world noise and artifacts: Although EEGPT learns from diverse datasets, it does not explicitly model or remove artifacts and noise
- Long-term session variability remains largely unresolved.