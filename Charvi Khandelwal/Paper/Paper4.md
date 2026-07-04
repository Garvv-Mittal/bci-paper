Computer-aided tumor diagnosis in automated breast ultrasound using 3D detection network

OBJECTIVE-

This paper aimed to develop an AI system that can automatically detect breast tumors in Automated Breast Ultrasound images and classify them as benign or malignant.

Usually, doctors have to manually examine hundreds of ultrasound images, which is time-consuming and may lead to missed tumors. Therefore, the researchers proposed an automated system to improve detection speed and accuracy.

METHODOLOGY-

The researchers proposed a two-stage 3D Detection Network. first, it detects the suspected tumor.,then, it classifies the tumor as benign or malignant.
A 3D Region Proposal Network with a 3D CNN backbone was used to detect lesion areas by using the spatial information present in 3D ultrasound images.
IoU-balanced Classification Loss was introduced to give more importance to predictions that correctly locate the tumor, improving detection accuracy.
Similarity Loss was used to make tumor features more distinguishable from background tissues, reducing confusion during detection.
A separate classification network was used after detection to improve the final prediction.
The proposed model was compared with existing methods like 2D U-Net, 3D U-Net and basic RPN models.
RESULTS

The proposed RPN-IOU-Sim model performed better than existing methods by achieving higher tumor detection accuracy with fewer false detections.
IOU-balanced Classification Loss and Similarity Loss both improved the model's ability to locate tumors and classify them correctly.
The model worked well for both small and large tumors and showed strong performance in detecting breast cancer from ABUS images.
THIS PAPER PROPOSES A TWO-STAGE 3D DETECTION NETWORK FOR AUTOMATIC BREAST TUMOR DETECTION AND CLASSIFICATION.

IT USES IOU-BALANCED CLASSIFICATION LOSS AND SIMILARITY LOSS TO IMPROVE DETECTION ACCURACY WHILE REDUCING FALSE POSITIVES.

THE MODEL HELPS DOCTORS BY MAKING BREAST CANCER DETECTION FASTER, MORE ACCURATE, AND LESS DEPENDENT ON MANUAL SCREENING.
