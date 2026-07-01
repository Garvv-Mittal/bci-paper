# EGA Model (Entropy Guided Adversarial Model)

**Title:** Entropy Guided Adversarial Model for Weakly Supervised Object Localization  
**Authors:** Sabrina Narimene Benassou, Wuzhen Shi, Feng Jiang  
**Journal:** Neurocomputing, Vol. 429 (2021), pp. 60–68  
**DOI:** [10.1016/j.neucom.2020.11.038](https://doi.org/10.1016/j.neucom.2020.11.038)

---

## Problem

Weakly Supervised Object Localization (WSOL) aims to identify an object's location in an image using only image-level labels, without relying on bounding-box annotations. Most WSOL methods are based on Class Activation Maps (CAMs) no bounding boxes. The standard trick is Class Activation Maps (CAM), but CAMs have a known flaw: the network only lights up the most discriminative part of the object (e.g. a bird's head) and ignores the rest of it (the body, wings, etc.). Earlier fixes were either erase parts of the image to force the network to look elsewhere, or restructure the network to pull CAMs from multiple layers. Both come with downsides erasing causes information loss and can make the model latch onto background clutter, while architecture changes are messy and hard to generalize.

---

## Approach

The authors propose the **EGA model**, which sidesteps both issues without touching the network architecture or removing any image content. 

1. **Adversarial examples as data augmentation** --> instead of training only on clean images, the model is also trained on adversarially perturbed versions of the same images. The intuition (backed by prior work on adversarial robustness) is that a network trained this way stops relying on just the single most "obvious" feature and starts picking up on broader, more meaningful patterns across the object basically more of the bird, not just its head.By training the model on adversarial data.

2. **Shannon entropy as a guiding loss** — CAM pixels belonging to the object tend to have low entropy (high confidence), while pixels outside the object are high entropy (low confidence). By adding an entropy minimization loss on the CAM, the model is pushed to become more confident over a larger area of the object, effectively expanding the activated region to cover more of it.

Architecturally, clean and adversarial examples are passed through the same backbone but with separate batch normalization branches (main BN for clean, auxiliary BN for adversarial), producing two CAMs 1)CAM_clean and 2)CAM_adv

---

## Results

- EGA achieved **state-of-the-art results** on standard WSOL benchmarks for both localization and classification accuracy.
- Ablation studies confirmed that both components matter and removing either the adversarial training or the entropy loss will significantly hurts the performance which showing they complement each other.
- Because no image regions are erased and the backbone is untouched therefore the method generalizes easily across different CNN architectures without redesign.

---

## Limitations

- Adversarial example generation adds extra computational cost during training (extra forward/backward passes per batch).Inshort expensive.
- The approach is still CAM-based so it inherits CAM's fundamental sensitivity to how the underlying CNN's last conv layer is structured.
- Evaluated mainly on standard classification-style benchmarks (e.g. CUB-like datasets) performance on more cluttered, multi-object, or video data isn't explored.
- Entropy minimization can be a double-edged sword — pushing confidence too aggressively risks expanding the activation into background regions if not carefully balanced.

---

## Future Scope

- Extending the entropy-guided adversarial idea to multi-object or cluttered scenes.
- The method can further improves localization by producing segmentation-like outputs, allowing it to capture the object's shape and extent more accurately than traditional bounding-box approaches.
- Applying the same entropy + adversarial framework to other weakly supervised tasks like detection or segmentation.

---

## Relevance to Our Project

- The **entropy loss trick** is lightweight and architecture-agnostic hence easy to bolt onto whatever backbone we're already using without redesigning anything.
- If we're dealing with limited annotations (no bounding boxes), this gives us a way to improve localization quality just from classification labels.
- The **adversarial training as augmentation** angle could also help with general robustness of our model, which is a nice side benefit beyond just localization.


---


