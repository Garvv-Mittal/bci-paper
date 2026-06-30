# Paper 4: Decoding Covert Speech From EEG — A Comprehensive Review

## Main Aim

This paper is a comprehensive review of the literature on imagined (covert) speech decoding using EEG. Rather than proposing a new model or experiment, it systematically summarizes and compares major studies from the previous decade, providing researchers with a unified overview of existing methods, challenges, and future research directions.

---

## Core Concepts & Technologies Used

### Covert (Imagined) Speech
- Refers to silently imagining speech without moving the lips, tongue, or vocal cords.
- Forms the basis of speech-based Brain-Computer Interfaces (BCIs) for individuals unable to speak.

### Comparison of Brain-Sensing Modalities

The review compares multiple brain imaging technologies based on their strengths and limitations:

| Modality | Advantages | Limitations |
|----------|------------|-------------|
| EEG | Non-invasive, portable, inexpensive | Low spatial resolution, noisy signals |
| ECoG | High spatial and temporal resolution | Requires brain surgery |
| fMRI | Excellent spatial resolution | Expensive and poor temporal resolution |
| fNIRS | Portable and non-invasive | Slower response time |
| MEG | High temporal resolution | Very expensive and non-portable |
| Intracortical EEG (ICE) | Highest signal quality | Highly invasive |

### Two-Streams Hypothesis & Dual Stream Prediction Model (DSPM)

These neuroscience models explain how the brain processes speech:

- **Ventral Stream:** Processes meaning and language comprehension.
- **Dorsal Stream:** Responsible for speech production and motor planning.

The review discusses how imagined speech shares characteristics with spoken speech while also exhibiting important differences.

### Feature Extraction & Classification Techniques

The paper summarizes commonly used methods across previous studies, including:

- Spectral features
- Wavelet-based features
- Time-frequency analysis
- Support Vector Machines (SVM)
- Artificial Neural Networks
- Deep Learning models

### PRISMA-Based Systematic Review

- Initially screened **504 research papers**.
- Narrowed the selection to approximately **28 core EEG imagined speech studies**.
- Followed a structured PRISMA methodology to ensure objective paper selection.

---

## Gist of the Paper

This paper serves as a comprehensive reference for researchers entering the field of imagined speech Brain-Computer Interfaces. It reviews how previous studies designed experiments, selected vocabularies, positioned EEG electrodes, preprocessed brain signals, extracted features, and trained machine learning models.

The review also examines the neuroscience behind imagined speech, discussing whether it is processed similarly to overt speech. The authors conclude that imagined and spoken speech share neural representations at the semantic (meaning) level but differ during phonological and motor planning stages.

Finally, the paper identifies the major research challenges preventing practical imagined speech BCIs and highlights the need for standardized evaluation methods and real-time systems.

---

## Key Findings

- EEG remains the most widely used modality because it is inexpensive, portable, and non-invasive.
- Most imagined speech studies are still performed **offline**, with very few real-time (online) systems available.
- No universally accepted combination of electrodes, preprocessing methods, features, or classifiers exists across studies.
- Imagined speech and spoken speech share neural overlap at higher semantic levels but differ at phonological and speech-production stages.
- Transfer learning from spoken speech to imagined speech appears promising but remains an open research challenge.

---

## Research Gaps

- As a review paper, it presents no new experimental results or accuracy comparisons.
- The field remains highly fragmented, making cross-study comparison difficult.
- There is no standardized benchmark dataset or vocabulary for imagined speech research.
- Very few studies focus on practical, real-time imagined speech decoding systems.

---

## Future Scope

- Develop practical real-time imagined speech BCIs suitable for everyday communication.
- Establish standardized benchmark datasets and evaluation protocols.
- Investigate transfer learning techniques between spoken and imagined speech.
- Improve signal processing and deep learning methods to enhance decoding accuracy and generalization.

---

## Key Takeaway

This review provides a comprehensive overview of the imagined speech decoding field, highlighting both its progress and its remaining challenges. It concludes that while EEG-based imagined speech decoding has advanced significantly, standardized benchmarks, larger datasets, and real-time online systems are still needed before Brain-Computer Interfaces can become practical communication tools.