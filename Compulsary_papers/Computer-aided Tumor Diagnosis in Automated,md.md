ABUS 3D Tumor Diagnosis - Study Notes

CORE CHALLENGE
Doctors struggle with Automated Breast Ultrasound (ABUS) because it creates massive 3D data blocks containing roughly 800 frames per volume. And one breast scan needs 3 volumes.
Manually screening these is slow and can lead to overlooked abnormalities 
Tumors are needles in a haystack, often taking up less than 1 percent of the total volume
Ultrasound image quality is lower than CT or MRI, making it hard to see clear boundaries and distinguish benign from malignant growths 

DETECTION 
The researchers moved away from traditional pixel-coloring (segmentation) and built a Two-Stage 3D Detection Network 
Stage 1 uses a 3D Region Proposal Network (RPN) to scan the volume and draw 3D boxes around suspicious areas.
The backbone consists of 5 Res-blocks designed to extract representative features from the 3D volume.(because we used 3d so its easier to analyze the tumor better)
The system uses 125 anchors per feature cell across 5 base sizes (8, 16, 28, 40, and 55) to catch tumors of all sizes.
By analyzing large 3D patches (320 x 96 x 320), the network uses full spatial context instead of isolated 2D slices.

IOU-BALANCED CLASSIFICATION LOSS
This math formula links localization accuracy (the box) with classification confidence (the diagnosis).
Standard AI can be confidently wrong about where a tumor is, but this system uses a weight coefficient based on Intersection-over-Union (IoU).
The network only gets a high confidence score if its predicted box perfectly overlaps with the actual lesion.
This forces the AI to be extremely precise about where it draws the tumor boundaries.

SIMILARITY LOSS
Benign and malignant tumors look nearly identical in ultrasound, both appearing as dark blobs.
The researchers converted each detected area into a feature vector, which is a mathematical string of numbers.
Similarity Loss uses Cosine Similarity to mathematically push benign features away from malignant features in the AI's memory.
This makes it much harder for the AI to confuse a safe growth with a dangerous one .

PERFORMANCE METRICS
Sensitivity: 97.66 percent (The model almost never misses a tumor).
False Positives: 1.23 per volume (Very few false alarms for a system this sensitive).
Classification Accuracy (AUC): 0.8720 .
Size Reliability: 100 percent detection rate for any tumor larger than 4 cm3.

KEY TAKEAWAYS
Using 3D detection instead of 2D segmentation is more efficient for large medical volumes
Weighting classification scores based on location accuracy (IoU) drastically reduces diagnosis errors
Mathematical vector separation (Similarity Loss) is a powerful tool for distinguishing between visually similar medical conditions