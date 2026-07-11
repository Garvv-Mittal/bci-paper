# Hidanshu — Literature Review, Compulsory Papers, and Pre-Reads

This folder collects all my Phase 1 contributions to the BCI group's literature review, organized around our shared goal of building a **low-latency imagined speech / thought-to-text** decoding system.

The content is split into three areas:

- **Chosen Papers** — six core papers I selected that directly address our problem statement.
- **Compulsory Papers** — four methods assigned to all members (EEGNet, Tumor Diagnosis, FBCNet, Entropy Model).
- **Pre-Reads** — two short conceptual documents on the normalisation and onset-detection techniques that underpin our proposed architecture.

---

## Folder Structure

```text
literature-review/Hidanshu/
├── Paper/
│   ├── Chosen Papers/
│   │   ├── README.md
│   │   ├── paper1_imagined-speech-eeg-deep-learning.md
│   │   ├── paper2_hybrid-spatial-temporal-eeg.md
│   │   ├── paper3_multi-receptive-cnn-async-bci.md
│   │   ├── paper4_delay-differential-analysis.md
│   │   ├── paper5_low-latency-edge-handwriting.md
│   │   └── paper6_eeg-conformer.md
│   └── Compulsory Papers/
│       ├── paper1_eegnet.md
│       ├── paper2_tumor-diagnosis.md
│       ├── paper3_fbcnet.md
│       └── paper4_entropy-model.md
└── Pre-reads/
    ├── pre-read1_adaptive-normalisation.md
    └── pre-read2_evidence-accumulation.md
```

All paper notes follow the repository's standard template: Paper Info → Summary → Key Contributions → Methodology → Results → Relevance to Our Project → Research Gaps / Open Questions.

---

## Chosen Papers (Imagined Speech + Low Latency)

Six papers selected because they collectively cover the full problem: onset detection, feature extraction, latency-accuracy trade-offs, and backend classification. See `Paper/Chosen Papers/README.md` for the full cross-paper analysis.

| # | Paper | Key Result |
|---|-------|------------|
| 1 | Imagined Speech Classification Using EEG and Deep Learning (Abdulghani et al., 2023) | 92.5% acc., 4-class, 8-channel EEG + WST + LSTM |
| 2 | Hybrid Spatial–Temporal Decoding of Imagined Speech (Alharbi & Alotaibi, 2024) | 77.8% acc., 5-phrase task, topographic maps + 3DCNN–RNN |
| 3 | Multi-Receptive CNN for Asynchronous Imagined Speech BCI (Ko et al., 2025) | Continuous onset detection ("brain switch") in real EEG |
| 4 | Delay Differential Analysis for Imagined Speech (Carvalho et al., 2024) | Lightweight non-DL features benchmarked against all public DL baselines |
| 5 | Low-Latency Neural Inference for Imagined Handwriting on an Edge Device (Sen et al., 2025) | 88.84% @ 202.6 ms/char on Jetson TX2 — the closest existing system to our goal |
| 6 | EEG Conformer (Song et al., 2023) | SOTA across 3 public datasets; CNN + self-attention backbone |

These six papers together reveal a clear gap: no existing system combines continuous onset detection + imagined speech decoding + real-time edge deployment in a single pipeline. That is the gap our architecture is designed to fill.

---

## Compulsory Papers

Four methods assigned to all members, summarised in `Paper/Compulsory Papers/`:

| # | Paper | Core Idea |
|---|-------|-----------|
| 1 | EEGNet (Lawhern et al., 2018) | Compact depthwise + separable CNN for EEG, generalizes across paradigms |
| 2 | Tumor Diagnosis (DOI: 10.1080/1206212X.2017.1313490) | Wavelet + entropy features for pathology detection; clinical BCI context |
| 3 | FBCNet (Mane et al., 2021) | Filter-bank multi-view CNN + Variance layer, SOTA on MI datasets |
| 4 | Entropy Model (DOI: 10.1080/1206212X.2017.1313490) | Entropy-based EEG features for MI BCIs; nonlinear complexity measures |

> **Note:** Papers 2 and 4 share the same DOI link as assigned; exact author lists and volume details are pending full text access. Placeholders are clearly marked inside those files.

These four papers provide the general-purpose EEG toolbox (compact CNNs, filter banks, entropy features, clinical applications) that we can adapt and compare against for our imagined-speech pipeline.

---

## Pre-Reads (Conceptual Groundwork)

Two documents written to establish a shared mental model for our dual-stage architecture before implementation begins:

**Pre-read 1: Adaptive Feature Normalisation for EEG**
Covers why raw EEG amplitudes cannot be thresholded directly (session drift, subject variability, electrode impedance), what Z-score normalisation does and how to apply it in a real-time setting, and why relative band power ratios are more stable than absolute power values. Explains how both feed into our CUSUM and DDM stages.

**Pre-read 2: Evidence Accumulation Models — CUSUM vs Drift–Diffusion**
Covers why single-shot classification is insufficient for continuous imagined speech detection, how CUSUM accumulates evidence for change detection and why it is a natural Stage 1 component, how Drift–Diffusion Models provide a richer parametric framework for the same process, and how they compare in complexity, interpretability, and edge deployability. Includes the full four-stage pipeline from feature extraction through to decision handoff to Stage 2.

---

## How This Fits the Group's Workflow

- **Phase 1 (this folder):** Literature review complete for chosen and compulsory papers.
- **Phase 2:** Dataset understanding — notes and statistics on our target imagined-speech dataset to be added here once assigned.
- **Phases 3–5:** EEGNet and FBCNet provide starting architectures; Paper 5's latency curve format guides evaluation metrics.
- **Phase 6:** The chosen paper notes and pre-reads feed directly into the introduction and methods sections of the group's research report.

If you want to add or modify anything in this folder, follow the repo's git rules: work on your branch (`hidanshu`), commit with a clear message, and open a PR into `main` for team lead review.
