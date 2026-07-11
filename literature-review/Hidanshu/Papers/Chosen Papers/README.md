# Hidanshu — Chosen Papers

**Problem statement:** Recent BCI work shows inner speech can be decoded, but accuracy and latency are still limited (~70% accuracy). The goal is a real-time neural decoding pipeline that converts brain signals into text with low latency and high accuracy, suitable for imagined speech.

This folder contains notes on six chosen papers that are directly relevant to our problem, plus cross-paper connections to our proposed dual-stage architecture:

- **Stage 1:** Adaptive feature normalization + evidence-accumulation (CUSUM / DDM) for onset detection and cross-session robustness.
- **Stage 2:** Deep classifier for decoding the content of imagined speech with good latency–accuracy trade-offs.

Each paper has a full note in its own `.md` file using the standard template: Paper Info → Summary → Key Contributions → Methodology → Results → Relevance to Our Project → Research Gaps / Open Questions.

---

## Paper List

| # | Paper | Key Number / Point | File |
|---|-------|--------------------|------|
| 1 | Imagined Speech Classification Using EEG and Deep Learning (Abdulghani et al., 2023) | 92.5% acc., 4-class, 8-channel EEG + WST + LSTM | `paper1_imagined-speech-eeg-deep-learning.md` |
| 2 | Decoding Imagined Speech from EEG Data: A Hybrid Deep Learning Approach (Alharbi & Alotaibi, 2024) | 77.8% acc., 5-word, topographic maps + 3DCNN–RNN | `paper2_hybrid-spatial-temporal-eeg.md` |
| 3 | Imagined Speech Detection Using Multi-Receptive CNN for Asynchronous BCI (Ko et al., 2025) | Onset/idle "brain switch" detection in continuous EEG | `paper3_multi-receptive-cnn-async-bci.md` |
| 4 | Decoding Imagined Speech with Delay Differential Analysis (Carvalho et al., 2024) | Non-DL, lightweight DDA features vs. all public DL baselines | `paper4_delay-differential-analysis.md` |
| 5 | A Low-Latency Neural Inference Framework for Real-Time Handwriting Recognition from EEG on an Edge Device (Sen et al., 2025) | 88.84% acc. @ 202.6 ms/char on Jetson TX2 | `paper5_low-latency-edge-handwriting.md` |
| 6 | EEG Conformer: Convolutional Transformer for EEG Decoding and Visualization (Song et al., 2023) | SOTA across 3 public datasets; CNN + self-attention backbone | `paper6_eeg-conformer.md` |

---

## How These Connect to Our Dual-Stage Architecture

### Stage 1 — Onset detection and feature normalization

**Paper 3** (multi-receptive CNN for asynchronous imagined speech) tackles exactly the same problem as our CUSUM / evidence-accumulation stage: detecting *when* imagined speech starts in a continuous EEG stream — the "brain switch." Once we have full text access, it becomes the direct benchmark for our onset-detection latency and false-positive/false-negative rates.

**Paper 4** (DDA) and **Paper 1** (WST) propose lightweight feature-extraction methods that can feed our normalization and accumulation stages. They are attractive because they reduce dimensionality and noise before deep models see the data, which is important for keeping the overall pipeline latency low.

### Stage 2 — Classification and representation

**Paper 2** converts EEG into sequences of topographic maps and uses a 3DCNN–RNN hybrid to capture spatial and temporal structure jointly. This is a candidate representation if we decide our normalization stage should operate on spatial maps rather than raw channels.

**Paper 6** (EEG Conformer) is a general-purpose CNN + Transformer architecture for EEG decoding across multiple paradigms — a strong reference for the classification stage after onset detection, though edge-latency characterization is still needed.

### Latency benchmarking and edge deployment

**Paper 5** (EEdGeNet) is the closest existing system to our performance target: real-time decoding on a physical edge device, with a latency–accuracy curve rather than a single-number accuracy. It demonstrates that ~90% accuracy at ~200 ms per character is achievable today for imagined handwriting. We plan to report our own imagined-speech results in the same format — accuracy as a function of latency budget — rather than a single accuracy number.

---

## Open Cross-Paper Gaps

- **Latency reporting is rare:** Only Paper 5 reports concrete on-device latency in milliseconds. None of the imagined-speech papers (1, 2, 3, 4, 6) report end-to-end latency, which makes it impossible to compare "accuracy" claims against our actual goal of real-time decoding.
- **No paper combines all three:** Continuous onset detection (Paper 3) + imagined speech content decoding (Papers 1, 2, 4, 6) + real-time edge deployment (Paper 5) have never been done in a single system. That combination is the gap our project aims to fill.
- **Small vocabularies, limited subjects:** Most imagined-speech papers here work with 4–5 classes and few subjects, with limited cross-subject evaluation. This motivates using larger datasets and focusing on generalization in our experiments.
