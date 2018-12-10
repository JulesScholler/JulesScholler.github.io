---
layout: page
title: ""
subtitle: "Predicting the arrow of time in videos"
---

### Project Summary

In this project we explore whether we can predict the arrow of time in a temporal sequence, i.e. if it is possible to tell whether a video is running forwards or backwards. We develop a framework using computer vision tools and machine learning to carry out this first part of the project. We then focus on the important types of motion features for predicting the arrow of time. The second part of the project aims at localizing them in the image sequences by exhibiting an influence map for the decision.

For a thorough explanation about this project you can get the report [here](/pdf/Report_RecVis.pdf).

### Predict the arrow of time

The first step to predict the arrow of time was to extract the motion between each frames. To do that we computed the optical flow (which is the pattern of apparent motion of objects, surfaces, and edges in a visual scene) using Lukas-Kanade Dog

$z = x + y$
