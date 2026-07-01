## Title EEG Conformer: Convolutional Transformer for EEG Decoding and Visualization

## Problem Addressed
- CNNs Only Learn Local Features
- CNNs have a limitation. They only look at a small receptive field.
- Important information may occur seconds apart. CNNs struggle to model these long-range dependencies.
- Transformers capture long range frequencies but transformers alone are not ideal
- Existing Models Ignore Both Local and Global Information Together
- Poor Model Interpretability
- Most deep learning EEG models behave like black boxes.
- Existing EEG decoding models fail to simultaneously capture local spatio-temporal information, long-range temporal dependencies, and model interpretability.

## Methodology
- Instead of feeding raw EEG directly into a Transformer (which is computationally expensive and less effective on small datasets), the model first applies CNN layers.
- Its purpose is local feature extraction.
- Raw EEG is: noisy, high-dimensional, difficult for Transformers to learn directly.
- The CNN helps by: removing redundancy, extracting meaningful low-level features, reducing the sequence length.
- This makes the  Transformer more effective.
- Patch Embedding
- EEG Feature Patch → Token
- Now the sequence of EEG tokens enters the Transformer. This is where global relationships are learned.
- The most important part of the Transformer is self-attention.
- For every EEG token, the model asks: "Which other EEG tokens are important for understanding this one?"
- This allows the model to capture long-range temporal dependencies.
- After the Transformer, the learned representation is passed to a classifier.
- The class with the highest probability becomes the prediction.
- EEG Conformer introduces Class Activation Topography (CAT) to explain its decisions.
- CAT visualizes:
    - Which EEG electrodes influenced the decision.
    - Which time regions were important.
    - Which features the Transformer attended to.
    - Instead of only the prediction 
- It improves interpretability.
- Researchers can verify whether the model is focusing on biologically meaningful brain regions rather than irrelevant noise.

## Key Results and Findings 
- Hybrid CNN + Transformer Performs Better Than Individual Models
- Better Spatio-Temporal Feature Learning
- Visualization (CAT) Demonstrated Biologically Meaningful Attention
- Better Generalization Than Traditional CNN Models

## Limitations 
- No Explicit Solution for Cross-Subject Generalization
- Requires Labeled Training Data
- Transformer Increases Computational Cost