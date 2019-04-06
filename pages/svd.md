---
layout: page
title: "Motion artifact removal and signal enhancement"
subtitle: "Toward in vivo DFFOCT and deeper imaging"
comments: true
---

You can find my preprint on [arXiv](https://arxiv.org/abs/1904.00810).

**D**ynamic **F**ull **F**ield **OCT** (DFFOCT) is very promising for 4D microscopy with diffraction limited resolution and also for eye imaging. In this paper I present two methods, (i) a **S**ingular **V**alue **D**ecomposition (SVD) based filtering method that automatically removes artifact arising from sample axial motion during the acquisition, and (ii) a new operator based on non-stationnarities to compute the dynamic in order to improve the **S**ignal to **N**oise **R**atio (SNR). While the first method is a great step toward in vivo DFFOCT imaging the second method allow deeper imaging in scattering media.

## SVD filtering for motion artifact removal

One drawback of DFFOCT lies in its main advantage, it has a nanometer sensitivity. This means that the slightest mechanical vibration will cause a temendous signal variation compared to the typical probed fluctuations. Indeed, the static sample reflexion coefficient is typical two order of magnitude higher than the metabolic fluctuation. This means that mechanical vibration can bury the signal of interests under high artifacts. This was is an issue in the lab because the setup is mounted on a sturdy optical bench that filter out most of the vibrations. Problems arise when clinicians try to use such system in very noisy environment and for in vivo imaging when these artifacts need to be filtered out in order to retrieve the metabolic image.

## Cumulative sum for deeper imaging

