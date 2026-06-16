## Entropy Guided Adversarial Model for Weakly Supervised Object Localization##
Sabrina Narimene Benassoua, Wuzhen Shib, Feng Jianga

# Problem Addressed
- Weakly Supervised Object Localization (WSOL) is a computer vision task where a model must identify the location of an object in an image using only image-level labels during training, without being given the object's bounding box.
- Without location annotations, the network must learn both classification and localization from limited supervision, often resulting in inaccurate object boundaries.
- A Class Activation Map (CAM) is a heatmap showing which image regions contribute most to a classification decision.
- For example, if a network classifies an image as a cat, the CAM might assign:
    - Cat face → high activation (close to 1)
    - Cat body → medium activation
    - Background → low activation (close to 0)
- Most WSOL methods use Class Activation Maps (CAMs). However, CAM usually focuses only on the most informative region instead of the entire object.
- Localization accuracy suffers because predicted bounding boxes cover only partial objects.
- Many previous methods remove the most discriminative object part so that the network is forced to look elsewhere.
- However, after erasing object regions, networks often start focusing on background elements rather than actual object parts.
- This produces noisy activation maps and inaccurate localization.
- Region-erasing approaches require one or more hyperparameters to decide: how much of the image to erase, where to erase, how aggressively to erase
- these hyperparameters: differ for each dataset and each model used for training
- The methods become difficult to generalize and reproduce across datasets.
- When parts of the image are removed, useful visual information is also lost.
- multiple CAMS require additional layers, modules, or blocks.
- uncertain regions during CAM analysis 
- Ideally, every pixel should be clearly classified as either: object or background
- However, many pixels receive intermediate values such as: 0.4, 0.5 etc these are called uncertain regions
- high entropy = high uncertainty
- CAMs are: over-confident on the most discriminative regions and under-confident on many other regions of the object.
- as a result objects are not fully localized 

## Methodology
- the aim is to improve object localization without modifying the CNN architecture and without using region erasing.
- An adversarial example is an input that has been modified by a very small, carefully designed perturbation that is almost invisible to humans but can significantly affect a neural network's predictions.
- Most papers study adversarial examples as a security problem. This paper uses them differently.
- Adversarial examples force the network to learn more features of an object instead of relying only on the most discriminative part.
- Instead of treating adversarial examples as attacks, they use them as a learning mechanism.
- The authors generate adversarial images using the gradient of the classification loss.
- Step 1: Adversarial Perturbation
    - A small perturbation is added
    - The paper generates images that look visually identical to humans.
    - The CNN, however, must learn more robust features.
- Step 2: Training 
    - Instead of training only on original images, it is done on toh the orginal and the adversarial image 
    - this encourages discovery of: secondary object parts and complementary discriminative regions instead of only the strongest feature.
- Step 3: Compute Entropy Map
    - H(p)=−plog(p)−(1−p)log(1−p)
    - p = CAM activation value for a pixel H(p) = entropy of that pixel
- Step 4: Entropy Minimization
    - The authors introduce an entropy-guided loss.
    - High Entropy → Reduce
    - Low Entropy → Keep
    - now the entire network is identified instead of only the discriminative areas

# Results
- The proposed EGA model achieves better localization performance compared with the baseline CAM model.
- more accurate bounding boxes
- activation maps become sharper
- uncertain regions are reduced
- localization accuracy increases further
- reduced background activation

# Relevance to Project
- robust and generalized feature representations.