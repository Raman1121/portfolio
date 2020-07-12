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

Due to the success of deep learning in medical imaging applications combined with the rapid developments in computer vision, several practices such as using weights of pretrained networks and models on the ImageNet dataset have become de-facto methods for the training procedure. In this paper, the authors have questioned the efficacy of these practices and provide a solid reasoning with the help of exhaustive experiments that there is a need to rethink these procedures when it comes to medical imaging.

Firstly, the authors start by listing out fundamental differences in the dataset used in medical imaging and ImageNet which are as follows -  
<ol>
  <li> Medical images have a rather confined region of interest as compared to the natural images in ImageNet which have a clear goal subject. </li>
  <li> Medical datasets are much smaller in size as compared to the ImageNet dataset. </li>
  <li> Properties such as background illumination, texture and size of images in the two categories of datasets are often very different. </li>
  <li> Medical tasks often have much fewer number of classes as compared to 1000 classes in ImageNet. </li> 
