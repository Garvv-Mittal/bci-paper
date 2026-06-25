## Computer-aided Tumor Diagnosis in Automated Breast Ultrasound using 3D Detection Network

---

## Paper Info

| Field | Details |
|-------|---------|
| **Title** |Computer-aided Tumor Diagnosis in Automated Breast Ultrasound using 3D Detection Network |
| **Authors** |Junxiong Yu , Chaoyu Chen , Xin Yang, Yi Wang, Dan Yan, Jianxing Zhang, and Dong Ni|

## Problems Addressed 
The paper addresses the challenges faced while using ABUS (automated breast ultrasound)
- **problem 1:** Huge amounts of data
    - A single ABUS scan contains a large number of image slices.
    - each slice must be examined individually 
    - this process is time consuming 
    - increases workload
- **problem 2:** Existing CAD systems rely on handcrafted features
    - CAD stands for computer aided diagnosis schemes 
    - manual extraction of features
- **problem 3:** 2D based analysis 
    - scans are processed as individual 2D slices 
    - A tumor is inherently a 3D structure.
    - this excludes information about volume, shape etc of the tumor
- **problem 4:** Detection and diagnosis should be unified
    - one netwrok should be proposed that does both tumor localization and classification
- **problem 5:** Poor image quality of ultrasounds when compared to other scans like MRI and CT scan
- **problem 6:** tumors are easy to miss
    - they have a very small lesion error 
    - most of the space is soccupied as background 
    - Benign and Malignant Lesions Look Similar
    - texture difference is very subtle 
- **problem 7:** false positives 
    - Normal tissue is incorrectly marked as cancer.
- **problem 8:** U-NET
    - most of the deep neural networks are based on U-Net architecture.
    - U-Net architecture consumes a lot of computing resource in the decode stage
    - only a small patch can be input into the network 

## Methodology
- Step 1: Feature Extraction
    - The ABUS volume enters a 3D CNN.
    - Height × Width × Depth
    - backbone consists of 5 Res-blocks 
    - these blocks learn about spatial context, boundary information, lesion shape and lesion texture 
- Step 2: 3D Region Proposal Network (RPN)
    - here the actual location of suspected lesion areas takes place 
    - 5 base sizes are used to detect different sizes lesion areas
    - 125 anchor = 125 different candidate boxes
    - this increases the probability of detection 
    - RPN consists of two branches: classification and regression branch
    - classfication finds if a lesion is loacted, regression finds out the exact location
    - classifcation predicts tumor or background?
    - regression predicts the 6 3D coordinates of the tumor
- Step 3: IoU- Balanced Classification Loss
    - e lesion area often only accounts for a small proportion of the entire image, and is very similar with the background area.
    - nromally the classification and regression branch work independently 
    - High confidence, Poor localization can occur 
    - IoU is a metric used in object detection to measure how well a predicted bounding box overlaps the actual (ground-truth) bounding box.
    - How accurately has the regression branch located the lesion?
    - IoU is calculated as:
        IoU = Area of Union / area of Overlap
        Overlap = common region between predicted box and ground truth box
        Union = total area covered by both boxes
    - The authors give more importance to anchors with higher IoU.
    - Anchors that classify correctly and localize accurately should contribute more to learning.
- Step 4: Similarity Loss
    - Intra-class variation is large (Benign tumors may look very different from one another)
    - Inter-class variation is small (Benign and malignant lesions often look similar)
    - A feature vector is considered only if: Anchor-Ground Truth IoU > 0.3 and Anchor-Anchor IoU > 0.2
    - similarity between two feature vectors is computed using cosine similarity
    - positive similarity is increased and negative similarity is decreased

- Finally, the detected candidates are passed to a dedicated classification network, and the outputs of both networks are combined to determine whether a lesion is benign or malignant.

## Key Results and Findings
- Proposed system beats u-net in senstivity
- IoU-Balanced Classification Loss Improves Performance
- Similarity Loss Further Improves Results
- Experimental results show that the proposed 3D detection scheme can achieve superior performance when using both IoU-balanced classification loss and similarity loss.
- even small lesions which are harder to detect had high detection rate

## Relevance to project 
- concepts of generalization 
- eeg signals: non stationarity, similar issue is faced here which is combatted with similarity loss