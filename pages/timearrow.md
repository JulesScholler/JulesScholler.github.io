---
layout: page
title: ""
subtitle: "Predicting the arrow of time in videos"
---

### Project Summary

In this project we explore whether we can predict the arrow of time in a temporal sequence, i.e. if it is possible to tell whether a video is running forwards or backwards. We develop a framework using computer vision tools and machine learning to carry out this first part of the project. We then focus on the important types of motion features for predicting the arrow of time. The second part of the project aims at localizing them in the image sequences by exhibiting an influence map for the decision.

For a thorough explanation about this project you can get the report [here](/pdf/Report_RecVis.pdf).

### Predict the arrow of time

The first step to predict the arrow of time was to extract the motion between each frames. To do that we computed the optical flow (which is the pattern of apparent motion of objects, surfaces, and edges in a visual scene) using Lukas-Kanade The Lucas-Kanade derivative of Gaussian method which takes two input images and divides them into smaller sections and assumes a constant velocity in each section. Then, it optimizes the optical flow problem with a constant velocity model in each section. In order to reduce the noise in the optical flow estimation, we introduced a threshold to put to zero the small velocity values. We thus obtained a sparse matrix containing a vector
$(V_x, V_y)$ in each pixel.

The idea is then to compress this information into a smaller object. Indeed, supposing the images contain  $n \times m$ pixels, we have a $2nm$ dimensional vector with which we want to tell whether it belongs to a "forward class" or to a "backward class". The curse of dimensionality precludes us from predicting directly the arrow of time in such a high dimensional space. We therefore need to extract some higher representation features from the optical flow to hope achieving accurate predictions of the arrow of time.
