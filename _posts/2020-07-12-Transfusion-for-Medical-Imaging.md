---
toc: true
layout: post
description: Explanation of the paper: "Transfusion: Understanding Transfer Learning for Medical Imaging".
categories: [medical imaging, model pretraining]
title: Transfusion: Understanding Transfer Learning for Medical Imaging.
---

# Transfusion: Understanding Transfer Learning for Medical Imaging 

For my undergraduate thesis, I chose to work with Dr. Tavpritesh Sethi at IIIT-Delhi at the intersection of computer vision and medicine, particularly medical imaging. During the project, I had to train several classification models with different strategies in terms of data augmentation, treating imbalance, model pretraining, etc. While experimenting with different pretraining methods, I observed some marked differences in the training dynamics of the network which got me interested in this particular field and led me to this [paper](https://arxiv.org/abs/1902.07208) by the Google Brain team.  

## Paper Explanation

Due to the success of deep learning in medical imaging applications combined with the rapid developments in computer vision, several practices such as using weights of pretrained networks and models on the ImageNet dataset have become de-facto methods for the training procedure. In this paper, the authors have questioned the efficacy of these practices and provide a solid reasoning with the help of exhaustive experiments that there is a need to rethink these procedures when it comes to medical imaging. In addition to this, authors also study the extent of feature reuse accompanied with different weight initialization schemes and provide a scheme of their own.

Firstly, the authors start by listing out fundamental differences in the dataset used in medical imaging and ImageNet which also serves as a major motivation for this study -  
<ol>
  <li> Medical images have a rather confined region of interest as compared to the natural images in ImageNet which have a clear goal subject. </li>
  <li> Medical datasets are much smaller in size as compared to the ImageNet dataset. </li>
  <li> Properties such as background illumination, texture and size of images in the two categories of datasets are often very different. </li>
  <li> Medical tasks often have much fewer number of classes as compared to 1000 classes in ImageNet. </li> 

These observations like confined region of interest, size, illumination and (possible) over-parameterization of models points to the fact that standard imagenet models and their pretrained weights might be sub-optimal in the medical imaging setting. 

## Experiments

The authors experiment with a variety of model architectures and pretraining schemes to study their affects in feature reuse and overall model performance. 

| Model Architecture  	| Description                                                                                     	|
|---------------------	|-------------------------------------------------------------------------------------------------	|
| **ImageNet Models** 	| Models such as ResNet50 and Inception which were originally developed for the ImageNet dataset. 	|
| **CBR Models**      	| Simple models (of varying sizes) which follow a progression of Conv2D-BN-ReLU layers                   	|

| Weight Initialization Scheme 	| Description                                                  	|
|------------------------------	|--------------------------------------------------------------	|
| **Random**                   	| Model weights initialized with Random Initialization         	|
| **Pretrained from ImageNet** 	| Models weights obtained from pretrained networks on ImageNet 	|

In these experiments, it was observed that smaller models (CBR) perform at-par with larger models such as ResNet despite significant differences in size and complexity. We see a similar outcome in experiments performed using random initialization. **These observations raise questions about de-facto practices of using models like ResNet with pretrained weights.**

## Notable Observations

<ul>
<li> Transfer Learning has a bigger effect with very small amounts of data. </li>
<li> Transfer primarily helps the large models and smaller models show little difference between transfer and random initialization. </li>

In order to study the effects on hidden representations, the authors used CCA similarity score between representations learnt by different initialization schemes. 

![]({{ site.baseurl }}/images/Transfusion/CCA-plot.PNG "Plotting CCA similarity score wrt different initialization schemes.")

The above plot reveals a major detail that **representations learnt with random initialization are much more similar to each other than those learnt with pretrained ImageNet weights for larger models,with less of a distinction for smaller models.** This means that in case of larger models (ResNet50), the representations learnt when trained with random weights are more similar to each other than representations learnt with pretrained weights. In case of smaller models (CBR), the representations learnt with the two initializations do not show much difference. *Hence, initialization has a more evident effect of larger models.*




