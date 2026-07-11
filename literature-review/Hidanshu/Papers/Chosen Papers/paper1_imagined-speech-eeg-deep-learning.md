## Imagined Speech Classification Using EEG and Deep Learning

---

## Paper Info

| Field | Details |
|-------|---------|
| **Title** | Imagined Speech Classification Using EEG and Deep Learning |
| **Authors** | Mokhles M. Abdulghani, Wilbur L. Walters, Khalid H. Abed |
| **Year** | 2023 |
| **Journal** | Bioengineering (MDPI) |
| **Volume/Article** | Vol. 10, Article 649 |
| **DOI** | [10.3390/bioengineering10060649](https://doi.org/10.3390/bioengineering10060649) |

---

## Summary

The authors build a small-vocabulary imagined-speech BCI using a low-cost 8-channel EEG headset with electrodes placed at scalp locations chosen to maximize signal quality with minimal hardware. Four healthy participants silently imagined four directional commands (up, down, left, right) in response to audio cues. Instead of feeding raw EEG into a deep network, the pipeline first applies a wavelet scattering transformation (WST) to shrink dimensionality and pull out stable, noise-resistant features, which are then classified with an LSTM-based recurrent network. The combination reaches 92.5% overall accuracy on the four-class task.

---

## Key Contributions

- Demonstrates that a cheap, sparse-electrode (8-channel) EEG setup can still support high-accuracy imagined-speech classification if electrode placement is chosen deliberately rather than using a dense standard montage.
- Uses wavelet scattering transformation as a lightweight, hand-crafted alternative to learned CNN feature extractors, reducing dimensionality before the deep model ever sees the data.
- Pairs WST features with an LSTM-RNN to model temporal structure in the imagined-speech signal, rather than treating each time window independently.
- Reports a full metrics suite (accuracy, precision, recall, F1) instead of accuracy alone, giving a more complete picture of classifier reliability.

---

## Methodology

- Hardware: low-cost 8-channel EEG headset, MATLAB 2023a for acquisition.
- Participants: 4 healthy adults (2 male, 2 female, ages 20–56).
- Task: audio cue → participant silently imagines saying one of four words/commands (up/down/left/right) without moving articulators.
- Feature extraction: wavelet scattering transformation applied per-command to filter and stabilize features while cutting dimensionality.
- Classifier: LSTM recurrent neural network trained on the WST features to predict the imagined command.

---

## Results

- Overall classification accuracy: **92.50%**
- Precision: 92.74% · Recall: 92.50% · F1-score: 92.62%

---

## Relevance to Our Project

This is a useful lower bound and baseline for what a minimal-channel, classical-feature-extraction-plus-RNN pipeline can achieve on a small vocabulary. For our dual-stage architecture (adaptive normalization + CUSUM-based change detection for imagined speech), WST is worth considering as a candidate feature-extraction step that sits before our normalization stage — it is much cheaper than a learned CNN front end, which matters if we care about end-to-end latency. It is also a good sanity-check reference: if our pipeline cannot beat ~92% on a comparably small vocabulary with more channels, something in the pipeline needs revisiting. For imagined speech commands on low-density headsets, this pipeline is a useful baseline; our CUSUM/DDM front end and any edge-friendly classifier should aim to match or exceed this ~92% accuracy on similar small vocabularies while also satisfying our latency constraints.

---

## Research Gaps / Open Questions

- Gap: The paper reports accuracy and full metrics but does not report end-to-end inference latency or on-device performance, which is critical for our low-latency thought-to-text goal. A pipeline that hits 92.5% accuracy but takes 2 seconds per decision is not useful in our context.
- Only 4 subjects — no cross-subject generalization is reported. Would the same electrode placement transfer to a new person without recalibration?
- The task is a 4-class command set, not open-vocabulary speech — worth noting when comparing accuracy numbers across papers with different class counts.
