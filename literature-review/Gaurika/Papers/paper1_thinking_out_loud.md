# Thinking Out Loud: An Open-Access EEG-Based BCI Dataset for Inner Speech Recognition

## Main Aim

The primary objective of this paper is to address a major limitation in EEG-based inner speech research—the lack of a large, publicly available dataset. The authors developed and released an open-access EEG dataset to enable researchers worldwide to build, compare, and improve inner speech decoding models.

## Core Concepts & Technologies Used

### Inner Speech
- Silent thinking of words without any physical speech or movement.
- Forms the basis of imagined speech Brain-Computer Interfaces (BCIs).

### EEG (Electroencephalography)
- Non-invasive technique that records electrical activity from the scalp.
- Excellent temporal resolution for real-time BCIs.

### High-Density EEG
- 136-channel EEG acquisition system.

### Experimental Paradigms
1. Inner Speech
2. Pronounced Speech
3. Visualized Condition

### BIDS Format
Brain Imaging Data Structure (BIDS) for standardized organization and sharing.

## Gist of the Paper

The authors recorded EEG signals from 10 participants imagining four directional words (Up, Down, Left, Right). They also collected spoken and visualized versions of the same commands. The cleaned dataset was released publicly through OpenNeuro along with open-source code, providing a benchmark for future research.

## Results

- CSP + LDA/SVM achieved approximately 50–70% accuracy.
- Chance level for four classes is 25%.
- The dataset itself is the paper's primary contribution.

## Research Gaps

- Only four words.
- Offline analysis only.
- No latency evaluation.
- No deep learning experiments.
- Small sample size (10 participants).
- Spanish-only commands.

## Future Scope

- Apply CNNs, EEGNet, and Transformers.
- Build a real-time inference pipeline.
- Measure latency.
- Expand vocabulary.
- Increase participant diversity.

## Key Takeaways

- Public benchmark dataset for imagined speech EEG.
- 136-channel recordings.
- Three recording paradigms.
- Standardized BIDS organization.
- Strong foundation for future BCI research.
