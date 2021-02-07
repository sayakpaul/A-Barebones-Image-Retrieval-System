# A Barebones Image Retrieval System

<div align="center"><img src="https://i.ibb.co/ZXtwJjV/Webp-net-resizeimage.png" width="100" height="100"></img></div>

This project presents a simple framework to retrieve images similar to a query image. The framework is as follows:
- Train a CNN model (**A**) on a set of labeled images with Triplet Loss (I used [this one)](https://www.tensorflow.org/addons/api_docs/python/tfa/losses/TripletSemiHardLoss).
- Use the trained CNN model (**A**) to extract features from the validation set.
- Train a kNN model (**B**) on these extracted features with k set to the number of neighbors wanted.
- Grab an image (**I**) from the validation set and extract its features using the same CNN model (**A**).
- Use the same kNN model (**B**) to calculate the nearest neighbors of **I**.


<div align="center"><img src="https://i.ibb.co/s9QhG6Y/Screen-Shot-2020-08-04-at-11-05-42-PM.png" width="700"></img></div>


I used the **Flowers dataset** for experiments. I tried the above approach to a scenario where I had only 184 examples from the Flowers dataset and it worked well.

Here's a sample result:
<div align="center"><img src="https://i.ibb.co/ZVrLT3b/image.png"></img></div>

## Training specifics

I fine-tuned pre-trained models for minimizing the Triplet Loss. I experimented with the following pre-trained models:
- VGG16
- MobileNetV2
- ResNet50
- BigTransfer (also referred to as BiT) which is essentially a ResNet but pre-trained on a larger dataset with additional modifications

While training with the first three models I used the following learning rate callback (from the Transformers paper) -

<div align="center"><img src="https://i.ibb.co/kSFRtGb/image.png"></img></div>

Code of this callback is referred from [here](https://nbviewer.jupyter.org/github/GoogleCloudPlatform/training-data-analyst/blob/master/courses/fast-and-lean-data-science/keras_flowers_gputputpupod_tf2.1.ipynb).

While fine-tuning the BiT model I used what is referred to as the BiT-HyperRule. BiT models come in [different variants](https://tfhub.dev/google/collections/bit/1), I used this variant - `m-r50x1` . Refer to [this blog post](https://blog.tensorflow.org/2020/05/bigtransfer-bit-state-of-art-transfer-learning-computer-vision.html) to know more about BiT and BiT-HyperRule.

## Visualization of the embedding space of a limited validation set

***(The models were trained on 184 examples)***

<div align="center"><img src="https://i.ibb.co/ZdrgY7B/image.png"></img></div>

## Training progress

***(The models were trained on 184 examples)***

<div align="center"><img src="https://i.ibb.co/w6G3Wp5/image.png"></img></div>


The improvements with BiT are quite prominent. This indeed suggests that bigger models like BiT can be _sample-efficient_. 

## A few observations

Consider the following results (although they come from the model fine-tuned from VGG16):

<div align="center"><img src="https://i.ibb.co/9TfFYvD/image.png"></img></div>

We see that tulips and roses are being treated similarly and so are dandelions and daisies. If we see there is indeed an overlap in between their shapes and textures and this is likely why this is happening. When dealing with problems where very few samples are available per class it's good to have very rich representative samples per class which are distinct and indicative of a given class.

## References

- Moindrot, Olivier. “Triplet Loss and Online Triplet Mining in TensorFlow.” Olivier Moindrot Blog, 19 Mar. 2018, https://omoindrot.github.io/triplet-loss.
- Chapter 4 code of the Practical DL Book (O'Reilly). https://github.com/PracticalDL/Practical-Deep-Learning-Book/tree/master/code/chapter-4.
- Kolesnikov, Alexander, et al. “Big Transfer (BiT): General Visual Representation Learning.” ArXiv:1912.11370 [Cs], May 2020. arXiv.org, http://arxiv.org/abs/1912.11370.
- BigTransfer (BiT): State-of-the-Art Transfer Learning for Computer Vision. https://blog.tensorflow.org/2020/05/bigtransfer-bit-state-of-art-transfer-learning-computer-vision.html.

## Different model weights

Available [here](https://github.com/sayakpaul/A-Barebones-Image-Retrieval-System/releases/tag/v0.1.0). 

## Feedback

Via GitHub issues. 
