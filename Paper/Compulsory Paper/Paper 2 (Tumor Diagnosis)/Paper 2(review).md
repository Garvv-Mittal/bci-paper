# Computer-Aided Tumor Diagnosis in Automated Breast Ultrasound Using 3D Detection Network

---

## Metadata

| Field | Details |
|-------|---------|
| **Title** | Computer-Aided Tumor Diagnosis in Automated Breast Ultrasound Using 3D Detection Network |
| **Authors** | Junxiong Yu*, Chaoyu Chen*, Xin Yang, Yi Wang, Dan Yan, Jianxing Zhang, Dong Ni (*equal contribution) |
| **Year** | 2020 |
| **Conference** | Medical Image Computing and Computer-Assisted Intervention — **MICCAI 2020** |
| **Published in** | Lecture Notes in Computer Science, Vol. 12266, Springer, Cham |
| **DOI** | [10.1007/978-3-030-59725-2_18](https://doi.org/10.1007/978-3-030-59725-2_18) |

---

## Problem Addressed

Automated Breast Ultrasound (ABUS) is a newer imaging technique that scans the whole breast in 3D great for catching tumors that hand-held ultrasound might miss. The problem? Each ABUS scan generates roughly **800 image slices**, and making a radiologist go through all of them manually is painfully slow and error-prone.

Three things make automatic detection especially hard here:

- **Image quality is rough** — ultrasound is noisier than MRI or CT, so lesion boundaries are blurry and hard to label.
- **Tumors are tiny** — lesion regions are often less than 1% of the total scan volume. Class imbalance is severe.
- **Benign and malignant tumors look very similar** — just detecting a lump isn't enough you need to classify it too, and the visual differences are subtle.

Most previous methods used segmentation networks (like U-Net) which are computationally heavy and need pixel-level labels.

---

## Methodology / Approach

The framework is a clean **two-stage pipeline**:

### Stage 1 — 3D Region Proposal Network (3D RPN)
- A **3D ResNet backbone** (5 residual blocks) extracts volumetric feature maps from the ABUS scan.
- A **3D RPN** scans these features and proposes candidate lesion bounding boxes. It uses **125 anchor shapes** across 5 size scales (8, 16, 28, 40, 55 voxels) to handle the wide range of tumor sizes.
- Two branches run in parallel inside the RPN: one predicts bounding box coordinates (regression), the other predicts if the box contains a lesion or background (classification).

### Stage 2 — Classification Network
- Candidate regions from Stage 1 are passed to a separate classification network that decides **benign or malignant?**
- This seperates detection from diagnosis keeping each task focused.

### Two Novel Loss Functions (the real contribution)

**IoU-balanced Classification Loss**
Standard cross-entropy treats all positive proposals equally even ones with poor spatial alignment. This loss weights each positive proposal by how well its bounding box overlaps the ground truth (IoU). Anchors that regress well get a higher gradient signal anchors with sloppy bounding boxes get suppressed.Therefore the classifier and the localiser stay in sync.

**Similarity Loss**
Inspired by contrastive learning: it pulls feature vectors from the same class closer together and pushes different classes apart. This is critical because in ABUS benign and malignant tumors can look nearly identical to the backbone the similarity loss forces the feature space to be more discriminative at the embedding level not just at the output layer.

---

## Key Results / Findings

Dataset: **418 patients** from a Chinese hospital — 145 benign tumors, 273 malignant.

| Metric | Value |
|--------|-------|
| **Sensitivity** | **97.66%** |
| **False positives per volume** | **1.23 FPs** |
| **AUC (benign vs malignant)** | **0.8720** |

Compared to prior ABUS CAD systems:

- Wang et al. 2018 (CNN + threshold loss) 95.12% sensitivity at 0.84 FPs(false positives) slightly fewer false positives while its sensitivity was lower than the proposed method.
- Moon et al. (3D CNN + focal loss) 95.3% at 6.0 FPs the proposed method gets better sensitivity and far fewer false positives simultaneously.


studies confirmed:
- Removing the similarity loss dropped sensitivity noticeably.
- Removing IoU-balanced loss caused the classifier and localiser to seperate.


The detection-based approach (vs segmentation) enabled the model to process **full 3D volumes** rather than small patches which is where the spatial context advantage comes in.

---

## Limitations

- **Single-centre dataset** --> All 418 patients came from one hospital in China. Scans from different machines, imaging protocols, or patient demographics may perform differently but this hasn't been taken to account.
- **Small dataset for a 3D problem** -->418 cases is modest for a volumetric 3D CNN. The model may not generalize without a much larger dataset.
- **AUC of 0.87 for malignancy classification** is decent but not clinically deployable on its own — radiologists typically expect much more specificity before acting on a CAD output.
- **No cross-machine / cross-scanner evaluation**: ABUS devices from different vendors (GE, Siemens, Philips) produce different image characteristics the model isn't tested under this distribution shift.


---

## Future Scope

- **Multi-centre validation** -->Testing on scans from multiple hospitals and ABUS devices would immediately clarify how robust the model actually is.
- **Domain adaptation across scanners** --> The inter-device distribution changes in ABUS is the imaging equivalent of inter-subject variability in EEG — methods like adversarial adaptation or normalization layers could address this.
- **Semi-supervised / weakly supervised learning** --> Getting pixel-level labels for 800-frame 3D scans is expensive. Using image-level or bounding-box-level weak labels could be used to get larger datasets.
- **Transformer backbones** --> Replacing the ResNet with a 3D Swin Transformer or similar architecture could better capture long-range spatial relationships across slices.


---

## Relevance to Our Project

>**Honest note**: This paper is from medical imaging (breast ultrasound), not EEG or BCI. The direct technical overlap is limited.Having said that there are a few transferable ideas worth noting.

| Concept from this paper | How it connects to our project |
|------------------------|-------------------------------|
| **Similarity loss** | The idea of explicitly pulling same-class embeddings together and pushing different classes apart is directly applicable to cross-subject EEG — we want subject-invariant class representations, and contrastive / similarity losses are one way to get there. |
| **Distribution shift across scanners** | The ABUS inter-scanner variability problem is structurally similar to the inter-session / inter-subject EEG non-stationarity problem. Solutions from one domain can inspire the other. |
| **Two-stage pipeline** | Separating coarse feature extraction from fine-grained classification (Stage 1 → Stage 2) echoes the logic of having a domain-agnostic encoder followed by a subject-adaptive classifier in BCI. |



---

