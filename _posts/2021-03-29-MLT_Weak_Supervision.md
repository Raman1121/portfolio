---
toc: true
layout: post
description: "Explanation of the paper: Multi-task weak supervision enables anatomically resolved abnormality detection in whole-body
FDG-PET/CT"
categories: [medical imaging]
title: "Multi-task weak supervision enables anatomically resolved abnormality detection in whole-body
FDG-PET/CT"
comments: true
---

The following paper by Eyuboglu et al. has recently appeared in Nature Communications. It describes an attention-based, multi-task CNN that detects and localizes abnormalities in whole-body FDG-PET/CT scans. The "weak-supervision" in this framework comes from the process of deriving annotations from a language model trained on free-text radiology reports (a part of the dataset). The authors report the following observations -

<ol>
    <li> Training in a multi-task fashion enhances the performance. </li>
    <li> Task-specific attention modules further improve the localization performance. </li>
    <li> Task-specific attention modules further improve the localization performance. </li>
</ol>

## Motivations
Limited availability of data has been a long-standing roadblock in the context of supervised learning. This has motivated researchers to look beyond strictly-supervised techniques such as self-supervision and weak-supervision. On a similar note, the authors address this challenge by devising a framework where they utilize data from another modality (text) is used to derive annotations.  
Labeled datasets of appropriate size and structure for several diagnostic tasks are hard to find. Moreover, it is a widely known fact that deep learning models fail to generalize to datasets from different distributions. Change in imaging scanner type, adoption of a different post-processing method, etc are some of the common ways which can influence the data distribution and render the model impractical.   
These challenges laid the foundation for this research. The authors have developed a framework which eliminates the need to manually annotate the dataset by inferring annotations from radiology free-text reports.  

In the next section, I will attempt my best to describe some of the major components of this work.

## Methodology

### Language Model
Instead of relying on the hard-coded rules (supervised annotations), the authors develop a model to predict if an anatomical region is mentioned in case of an abnormal finding. This model is based on well-known BERT architecture.  
Instead of relying on the hard-coded rules (supervised annotations), the authors develop a model to predict if an anatomical region is mentioned in case of an abnormal finding. This model is based on well-known BERT architecture.  
Another interesting step that the authors perform is making BERT more specific to the medical (radiology) domain. BERT is trained on generic English text. In order to prevent the splitting of specialized, domain-specific terms in the reports, the authors develop a greedy algorithm to find the set of 3000 wordpieces that minimize the number of tokens required to reconstruct the reports in our training dataset. Out of these 3000, the words which were not present in BERT's vocabulary were added by replacing the “unusedX” and non-ASCII tokens provided in the BERT implementation. This allowed them to leverage BERT's original weights while adding domain-specific tokens.  

### Creating a Regional Ontology

The authors constructed an ontology of 94 regions, expressed as a directed acyclic graph. This contains both high level regions like "chest", "pelvis", "neck" along with fine-grained regions like "left lung", "upper lobe of right lung". The directed graph is used to establish relationship between these high-level and fine-grained regions. For instance, regions such as "Hilar Lymph Node" would come point towards "Thoracic Lymph Node" which would further point towards the high-level region "Chest". As we traverse along these edges, we move from fine-grained regions to higher-level regions.  

![]({{ site.baseurl }}/images/MLT_WS/ontology.PNG "Figure 1. A graph describing regional ontology relationships [Taken from the original paper]")

### Multi-Task learning with Attention

Multi-task learning (MLT) was an idea advocated by Rich Caruana in the late 90s. In MLT, our network attempts to solve several related tasks through shared representations. MLT has significantly matured as an active area of research. This [article](https://ruder.io/multi-task/) by Sebastian Rudder is an excellent resource to learn more about the field.  
The authors of this paper describe a MLT-inspired network. They perform extensive ablation studies which show that MLT network performs much better than a standard Single Task Network. The model used was a 3D Convolutional Neural Network (CNN) with 26 binary classification task heads and a shared CNN encoder.  
The MLT framework gave a superior performance in several aspects. Classes and anatomical regions with a very few samples (rare diagnoses) were identified with much higher accuracy and confidence using this approach.  
In addition, the authors infuse attention modules on each of the 26 binary classification heads which offered the following advantages - 
<ul>
    <li> Improvement in mean AUROC over all 26 tasks </li>
    <li> Better model interpretability by projecting model's attention distribution to the original image </li>
</ul>

### Concluding Thoughts

The paper was written in a clear language making it easy to read and understand. Some of the unique ideas that had an impact on me are the following - 

<ol>
    <li> **Multi Task Learning** - I have seen MLT models performing better in some cases. Here, the performance gain is quite significant. This has motivated me to experiment with different MLT approaches for my own experiments (Kaggle and Research) </li>

    <li> **Soft Attention** - I have always been a strong advocate of incorporating attention mechanisms particularly for medical imaging problems. This is because medical images have a bodily region of interest (ROI) as compared to natural images having a more global ROI. I am happy to see my thoughts having some practical value. </li>

    <li> **Weak Supervision** - One of my own research projects involved weak supervision. I always believed that it offers only marginal advantage. However, this work has shown that weak supervision could offer substantial performance gains given that we have unstructured data present. This is something I would like to try for future Kaggle competitions. </li>
</ol>


