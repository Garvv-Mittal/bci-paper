## EEG Conformer: Convolutional Transformer for EEG Decoding and Visualization

---

## Paper Info

| Field | Details |
|-------|---------|
| **Title** | EEG Conformer: Convolutional Transformer for EEG Decoding and Visualization |
| **Authors** | Yonghao Song, Qingqing Zheng, Bingchuan Liu, Xiaorong Gao |
| **Year** | 2023 (early access 2022) |
| **Journal** | IEEE Transactions on Neural Systems and Rehabilitation Engineering |
| **Volume/Article** | Vol. 31, pp. 710–719 |
| **DOI** | [10.1109/TNSRE.2022.3230250](https://doi.org/10.1109/TNSRE.2022.3230250) |

---

## Summary

Plain CNNs are good at picking up local temporal patterns in EEG but their limited receptive field makes them weak at capturing long-range dependencies across a signal. This paper proposes EEG Conformer: a compact architecture that bolts a self-attention (Transformer-style) module onto a CNN front end, so the CNN handles low-level local temporal and spatial features while the attention module models global relationships across the whole sequence. A lightweight fully-connected classifier sits on top. The authors also build in an interpretability tool — projecting class activation maps back onto brain topography — so you can see where on the scalp the model is basing its decisions. Tested across three public datasets spanning motor imagery and emotion recognition, it reports state-of-the-art results and has become a widely-cited general-purpose EEG decoding baseline, with code publicly released.

---

## Key Contributions

- Combines local convolutional feature extraction with global self-attention in a single compact model, rather than choosing one or the other.
- One-dimensional temporal and spatial convolution layers specifically designed for EEG's structure (channels × time), not a generic image-CNN adapted after the fact.
- Built-in interpretability: class activation mapping visualized on brain topography, so decisions can be sanity-checked against known neurophysiology.
- Validated across three different public datasets and two different task types (motor imagery and emotion recognition), suggesting the architecture generalizes across BCI paradigms.
- Open-sourced code, which has made it a common baseline and reference point in later EEG decoding papers.

---

## Methodology

- Convolution module: 1D temporal convolution followed by spatial convolution, extracting low-level local features across time and across electrode channels.
- Self-attention module: connected directly after the convolution module to model global correlations across the local features (the "Conformer" combination of Convolution + Transformer).
- Classifier: simple fully-connected layers on top of the combined representation.
- Interpretability: class activation mapping projected onto brain topography maps to visualize which scalp regions drove a given classification.
- Evaluation: three public EEG datasets covering motor imagery and emotion recognition paradigms.

---

## Results

Reported as state-of-the-art across all three public benchmark datasets tested. Exact per-dataset accuracy tables are in the full paper — worth pulling directly since "state-of-the-art" alone is not a comparable number against the other papers' specific accuracy figures in this review.

---

## Relevance to Our Project

EEG Conformer is a strong reference architecture for the classification half of our pipeline — after our adaptive normalization and CUSUM-based change detection have identified and cleaned a segment of imagined speech, a CNN + attention hybrid like this is a reasonable choice for the actual decoding step. It is designed for exactly this signal structure (channels × time) and has been validated across multiple BCI paradigms rather than one narrow task. The open-source code also makes it easy to drop into our pipeline as a baseline classifier to compare against simpler options like EEGNet. For our thought-to-text system, EEG Conformer is a candidate Stage 2 classifier, but its edge-deployment profile has not been characterized — we need to measure its inference latency directly on our target hardware and compare it against EEdGeNet before deciding whether its extra capacity is worth the computational cost.

---

## Research Gaps / Open Questions

- Gap: The paper does not report end-to-end inference latency or on-device performance, which is the most critical missing number for our low-latency goal. A model that is state-of-the-art in accuracy but takes 500 ms per decision does not fit our pipeline.
- Validated on motor imagery and emotion recognition, not imagined speech specifically — needs to be tested on imagined-speech datasets before we can treat its accuracy numbers as relevant to our task.
- How does the self-attention module's compute cost scale with longer input windows, which matters if our CUSUM stage produces variable-length detected segments rather than fixed-length trial windows?
