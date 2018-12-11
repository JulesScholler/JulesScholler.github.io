---
layout: page
title: ""
subtitle: "Predicting the arrow of time in videos"
comments: true
---

### Project Summary

In this project we explore whether we can predict the arrow of time in a temporal sequence, i.e. if it is possible to tell whether a video is running forwards or backwards. We develop a framework using computer vision tools and machine learning to carry out this first part of the project. We then focus on the important types of motion features for predicting the arrow of time. The second part of the project aims at localizing them in the image sequences by exhibiting an influence map for the decision.

For a thorough explanation about this project you can get the report [here](/pdf/Report_RecVis.pdf).

### Predict the arrow of time

The first step to predict the arrow of time was to extract the motion between each frames. To do that we computed the optical flow (which is the pattern of apparent motion of objects, surfaces, and edges in a visual scene) using Lukas-Kanade The Lucas-Kanade derivative of Gaussian method which takes two input images and divides them into smaller sections and assumes a constant velocity in each section. Then, it optimizes the optical flow problem with a constant velocity model in each section. In order to reduce the noise in the optical flow estimation, we introduced a threshold to put to zero the small velocity values. We thus obtained a sparse matrix containing a vector
$(V_x, V_y)$ in each pixel.

The idea is then to compress this information into a smaller object. Indeed, supposing the images contain  $n \times m$ pixels, we have a $2nm$ dimensional vector with which we want to tell whether it belongs to a "forward class" or to a "backward class". The curse of dimensionality precludes us from predicting directly the arrow of time in such a high dimensional space. We therefore need to extract some higher representation features from the optical flow to hope achieving accurate predictions of the arrow of time. To do so we used a pre-trained (vgg-f) **C**onvolutional **N**eural **N**etwork (CNN) and kept the output of the last layers (accuracy are compared in the report for different configurations).

In order to distinguish forward and backward motion features we trained a linear **S**upport **V**ector **M**achine (SVM). SVM are simple machine learning algorithms that given a set of training examples builds a model that assigns new examples to one category or the other. We yielded an accuracy of 90% which is quite high for this type of task. The most difficult video for time direction prediction was a flag moving due to the wind. Locally, the motion of the flag is harmonic and it is therefore difficult to redict, even for a human, the time direction. Easy videos were the one containing diverging motion (e.g. splash in swimming pool), indeed the algorithm learnt to classify those as forward videos. Interestingly it is as if the CNN learnt the $2^{nd}$ law of thermodynamics, stating that the entropy can only increase over time for an isolated system.

### Localizing key motions in videos for time direction prediction

Once achieved a satisfactory prediction accuracy for time direction prediction, a natural extension is to understand what the system has learnt. In order to do this, we drew inspiration from [1], which proposed a framework for building class activation maps, a representation that exposes the implicit attention of CNNs on an image. Doing that we obtained attention map that showed where the CNN was focusing in order to predict the arrow of time.

[1] B. Zhou, A. Khosla, A. Lapedriza, A. Oliva and A. Torralba, "Learning Deep Features for Discriminative Localization," 2016 IEEE Conference on Computer Vision and Pattern Recognition (CVPR), Las Vegas, NV, 2016, pp. 2921-2929. doi: 10.1109/CVPR.2016.319

