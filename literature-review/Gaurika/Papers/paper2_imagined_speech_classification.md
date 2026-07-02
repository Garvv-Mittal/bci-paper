# Paper 2: Imagined Speech Classification Using EEG and Deep Learning

## Main Aim

Develop a practical imagined speech decoder using a low-cost, consumer-grade EEG headset with only 8 channels and modern deep learning techniques. The goal is to demonstrate that high classification accuracy can be achieved without expensive laboratory-grade EEG systems.

---

## Core Concepts & Technologies Used

### Unicorn Hybrid Black+ Headset
- Consumer-grade EEG headset with **8 channels**.
- Significantly cheaper than research-grade EEG systems using over 100 channels.

### Strategic Electrode Placement
- Electrodes are positioned over:
  - **Broca's Area** – responsible for speech production.
  - **Wernicke's Area** – responsible for speech comprehension.
- This targeted placement captures language-related brain activity efficiently despite having fewer electrodes.

### Wavelet Scattering Transformation (WST)
- Signal processing technique used to extract stable and informative EEG features.
- Reduces noise while preserving important characteristics of the EEG signal.
- Produces compact feature representations suitable for deep learning.

### LSTM-RNN (Long Short-Term Memory Recurrent Neural Network)
- Deep learning architecture designed for sequential and time-dependent data.
- Learns temporal patterns present in EEG signals effectively.

### Audio-Cue Paradigm
Participants listened to the question:

> "Where do you want to go?"

They then imagined saying one of four commands:
- Up
- Down
- Left
- Right

This approach is more natural than traditional visual cue-based experiments.

---

## Gist of the Paper

This paper presents a practical imagined speech classification system built using an affordable 8-channel EEG headset. Instead of relying on expensive laboratory equipment, the researchers strategically positioned electrodes over the brain's language-processing regions to capture speech-related neural activity.

Raw EEG signals were processed using the **Wavelet Scattering Transformation (WST)** to generate clean and compact feature representations. These features were then fed into a **three-layer LSTM neural network**, which learned the temporal patterns associated with imagined speech commands.

The study demonstrates that combining strategic electrode placement, advanced signal processing, and deep learning enables high classification accuracy while keeping hardware costs low.

---

## Results

| Metric | Value |
|--------|-------|
| Overall Accuracy | **92.50%** |
| Precision | **92.74%** |
| Recall | **92.50%** |

### Key Findings

- Achieved **92.50% classification accuracy** using only **8 EEG channels**.
- Outperformed several traditional machine learning methods (approximately 70% accuracy).
- Demonstrated that low-cost EEG hardware can still achieve competitive performance.

---

## Research Gaps

- Only **4 participants** were included, limiting generalization.
- No proper **cross-subject validation** was performed.
- Vocabulary was limited to only **four directional commands**.
- Entire system was evaluated **offline**, with no real-time implementation or latency analysis.
- No benchmarking against the widely used **Nieto et al.** dataset, making direct comparison difficult.

---

## Future Scope

- Validate the WST + LSTM approach on larger public EEG datasets.
- Perform leave-one-subject-out validation to measure true generalization.
- Develop a real-time streaming imagined speech decoding pipeline.
- Expand the vocabulary beyond four commands toward phrase-level and phoneme-level decoding.

---

## Key Takeaway

This paper shows that accurate imagined speech classification does not require expensive high-density EEG systems. By combining strategic electrode placement, Wavelet Scattering Transformation, and LSTM networks, the authors achieved over **92% accuracy** using an affordable 8-channel headset, making practical Brain-Computer Interface (BCI) applications more accessible.