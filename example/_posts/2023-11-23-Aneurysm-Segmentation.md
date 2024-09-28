---
layout: post
title: Aneurysm Segmentation
image:
  path: /assets/img/blog/Sustainability.jpg
description: >
  Healthcare and AI: Precise segmentation of 3D MRI images containing aneurysms using 3D-Unet
image: /assets/img/blog/aneurysm.jpg
sitemap: false
---

## Introduction

An intracranial aneurysm (IA) is a weakened or thinned portion of a blood vessel in the brain that bulges dangerously and fills up with blood, which can compress surrounding nerves and brain tissue and have a high risk of rupture.

Over the last decade, many extraction algorithms have been designed by calculating local geometric features; however, rule-based methods have high computational costs and time requirements, and their performance is limited because of the wide variety of aneurysm shapes.

Deep learning techniques are becoming popular in medical image processing and one proposed model takes a surface model of an entire set of principal brain arteries containing aneurysms as input and returns aneurysm surfaces as output.

One way to segment 3D dataset is just by looking at one 2d Slice at a time, training a 2D unet, predicting each 2d slice and combine the volume together, which works fine in most cases, but not be the ideal choice for a lot of 3D data set because we probably need information from the few planes below and above. Our proposed model takes a surface model of an entire set of principal brain arteries containing aneurysms as input and returns aneurysm surfaces as output.

## Objective

Precise segmentation of pre-detected aneurysms: The model we propose takes as input a surface model of a complete set of main cerebral arteries (3D MRI data) containing aneurysms and returns the aneurysm surfaces as output.

## Dataset and Challenges

1. Working on real-life data, fresh from the hospital: 105 scans (3D MRI data)
   This is one of the main challenge: the low number of training data, and the complexity and size of the data (about 3cm x 7cm x 7cm) we have.
2. The input images are 192 by 192 by 64, we cannot load all of that into the memory.

## Preprocessing Data

- Simple augmentation that preserves the physical aspect of the data:

**Offline Augmentation**: Offline augmentation can dramatically increase the size of the dataset: Flipping, rotation
**Online Augmentation**: changing the sliding axis from xyz to xzy or yzx
<img src="/assets/img/blog/onlineaug.png" alt="drawing" width="500"/>

- We then divide these into patches of 64 by 64 by 64: The resolution of input 3D data is the primary reason behind the need for large memory and higher computation complexity in training a 3D CNN segmentation model. However, this could be circumvented with a patch-based approach. In patch-wise approach, the input 3D images are converted to small 3D sub-samples and analyzed individually. Finally, the patch-wise prediction outcomes will be merged to create the final 3D prediction map. The methodology uses cropped 3D patches of size (64 × 64× 64). The patch-based segmentation helps to reduce the hardware resources for training and generates a large number of data samples that favor adequate learning. However, the patch-wise analysis often fails to extract global features from the actual image volumes. This may limit the learning performance when the abnormality is region-specific.

- Randomly eliminate some patches that do not contain an aneurysm at all, To balance the number of training samples with and without aneurysms. Many of these patches remain in the data because they are considered useful for knowing when there is an aneurysm and when there is not.

- Image generator method
  This amount of data and its size (3D) is not possible to hold into memory so we will be using a generator method that will yield batches. When encountering a problem where the dataset used is too big to be loaded into memory at once that is being run on RAM. For example, we won’t be able to load all those images into memory before training.So, if we create a data generator, we can read images on the go when they will be used for training. Since we are reading the images on the go, we are saving memory.

we train the model on these patches, we load the next volume , then train the model then so on.
<img src="/assets/img/blog/preprocessing.png" alt="drawing" width="500"/>

## Metrics and Loss

<img src="/assets/img/blog/metricsandloss.png" alt="drawing" width="500"/>

## Modeling

The 3D U-Net architecture is quite similar to the U-Net.
It comprises of an analysis path (left) and a synthesis path (right).
In the analysis path, each layer contains two 3×3×3 convolutions each followed by a ReLU, and then a 2×2×2 max pooling with strides of two in each dimension.
In the synthesis path, each layer consists of an up-convolution of 2×2×2 by strides of two in each dimension, followed by two 3×3×3 convolutions each followed by a ReLU.
Shortcut connections from layers of equal resolution in the analysis path provide the essential high-resolution features to the synthesis path.
In the last layer, a 1×1×1 convolution reduces the number of output channels to the number of labels which is 3.
batch normalization (\BN”) before each ReLU.
19069955 parameters in total.

<img src="/assets/img/blog/3Dunet.png" alt="drawing" width="500"/>

When trying with a 3D_unet from scatch, we got fluctuations in the training loss.
Transfer learning: We can define backbones (ResNet50) that have pre-trained weights for faster and better convergence for the Unet.

<img src="/assets/img/blog/transferlearning.png" alt="drawing" width="500"/>

# Prediction

Instead of doing ensemble learning with different models, we have predict the 3D MRI test images with different rotation of the axis (changing the axis of the layers of the 3D MRI images), reshape them then average the transformed predictions using simple average. This give better results than predicting one version of the input test.

[Click to be directed to the code]

[Click to be directed to the code]: https://github.com/pierresamaha1998/Data-Challenge-Aneurysm-Segmentation
