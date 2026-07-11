## A Low-Latency Neural Inference Framework for Real-Time Handwriting Recognition from EEG Signals on an Edge Device

---

## Paper Info

| Field | Details |
|-------|---------|
| **Title** | A Low-Latency Neural Inference Framework for Real-Time Handwriting Recognition from EEG Signals on an Edge Device |
| **Authors** | Ovishake Sen, Raghav Soni, Darpan Virmani, Akshar Parekh, Patrick Lehman, Sarthak Jena, Adithi Katikhaneni, Adam Khalifa, Baibhab Chatterjee |
| **Year** | 2025 |
| **Journal** | Scientific Reports (Nature Portfolio) |
| **Volume/Article** | Article s41598-025-24972-y |
| **DOI** | [10.1038/s41598-025-24972-y](https://doi.org/10.1038/s41598-025-24972-y) |

---

## Summary

This is the closest paper in the set to our project's actual goal statement: real-time, low-latency, character-level decoding from EEG deployed on genuinely constrained edge hardware (an NVIDIA Jetson TX2), not just a fast model benchmarked offline. Participants imagined handwriting individual lowercase letters (a–z) while wearing a 32-channel EEG cap. The authors extract a large bank of time-domain, frequency-domain, and graphical features, then use Pearson-correlation-based feature selection to trim that bank down before feeding it into a hybrid Temporal Convolutional Network + MLP model they call EEdGeNet. The headline result is a full latency/accuracy tradeoff curve: the full-feature configuration is the most accurate but slowest, while a 10-feature subset cuts inference latency by over 4x for under 1% accuracy loss.

---

## Key Contributions

- First reported real-time, high-accuracy imagined-handwriting decoder actually deployed and benchmarked on a portable edge device, not just simulated offline.
- EEdGeNet: a hybrid TCN + MLP architecture designed specifically to be lightweight enough for edge inference while still modeling temporal structure in EEG.
- Pearson-correlation-based feature selection that explicitly optimizes for the latency/accuracy tradeoff rather than accuracy alone — directly reduces the 85-feature set down to a handful of high-value features.
- Reports concrete, device-level latency numbers (milliseconds per character) rather than abstract FLOPs or parameter counts, which is what actually matters for a real-time thought-to-text claim.

---

## Methodology

- Hardware: 32-channel EEG headcap; 15 participants.
- Task: silently imagine handwriting each lowercase letter a–z, plus a "do nothing" resting-state class.
- Preprocessing: bandpass filtering + artifact subspace reconstruction (ASR) to clean raw EEG.
- Feature extraction: 85 time-domain, frequency-domain, and graphical features per segment.
- Feature selection: Pearson correlation coefficient ranking, used to progressively reduce to smaller feature subsets.
- Model: EEdGeNet — a hybrid Temporal Convolutional Network (TCN) + multilayer perceptron (MLP) trained on selected features.
- Deployment: inference benchmarked directly on an NVIDIA Jetson TX2 edge device.

---

## Results

- Full 85-feature set: **89.83% ± 0.19% accuracy**, 914.18 ms per-character inference latency.
- Reduced 10-feature subset: **88.84% ± 0.09% accuracy**, 202.62 ms per-character latency — a **4.51× latency reduction** for under 1% accuracy loss.

---

## Relevance to Our Project

This paper is the best direct benchmark for our project's success criteria: it gives a concrete existence proof that ~90% accuracy at ~200 ms per character is achievable on real, low-power edge hardware today. Two things are directly transferable to our dual-stage architecture: (1) the Pearson-correlation feature-selection approach as a template for how we might prune whatever features feed into our adaptive normalization stage, and (2) their explicit latency-vs-accuracy curve as the format for reporting our own results. For our thought-to-text system, this paper sets a concrete target: ~90% accuracy at ~200 ms per symbol on real edge hardware; our imagined-speech pipeline should report comparable latency/accuracy curves rather than single accuracy numbers, and explicitly justify where we land on that curve relative to this baseline.

---

## Research Gaps / Open Questions

- Gap: This paper explicitly reports end-to-end on-device latency, which puts it ahead of every other paper in this set — but the task is imagined handwriting, not imagined speech. The feature-selection and edge-deployment approach may transfer, but requires re-validation on speech-imagery data specifically.
- 202.6 ms/character is their "fast" configuration — we need to define our own latency budget explicitly and compare against this number before claiming our pipeline is "low-latency."
- Would the same pipeline hit these latency numbers on cheaper or lower-power hardware than the Jetson TX2, which is a relatively capable edge board?
