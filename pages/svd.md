---
layout: page
title: "Motion artifact removal and signal enhancement"
subtitle: "Toward in vivo DFFOCT and deeper imaging"
comments: true
---

You can find our preprint on [arXiv](https://arxiv.org/abs/1904.00810).

**D**ynamic **F**ull **F**ield **OCT** (DFFOCT) is very promising for 4D microscopy with diffraction limited resolution and also for eye imaging. In this paper we present two methods, (i) a **S**ingular **V**alue **D**ecomposition (SVD) based filtering method that automatically removes artifact arising from sample axial motion during the acquisition, and (ii) a new operator based on non-stationnarities to compute the dynamic in order to improve the **S**ignal to **N**oise **R**atio (SNR). While the first method is a great step toward in vivo DFFOCT imaging the second method allow deeper imaging in scattering media.

## SVD filtering for motion artifact removal

### Theoretical considerations

One drawback of DFFOCT lies in its main advantage, it has a nanometer sensitivity. This means that the slightest mechanical vibration will cause a temendous signal variation compared to the typical probed fluctuations. Indeed, the static sample reflexion coefficient is typical two order of magnitude higher than the metabolic fluctuation. This means that mechanical vibration can bury the signal of interests under high artifacts. This was is an issue in the lab because the setup is mounted on a sturdy optical bench that filter out most of the vibrations. Problems arise when clinicians try to use such system in very noisy environment and for in vivo imaging when these artifacts need to be filtered out in order to retrieve the metabolic image. 

So basically when we acquire a DFFOCT stack we end up with the summation of our signal of interest *- which is small-* and the signals arising from motion artifacts. Our goal is then to separate those two contributions in order to retrieve only the dynamic.

### Proposed algorithm

In order to un-mix the artifactual signals we used the SVD. The first step is to unfold the 3D $M(x,y,t)$ cube of data into a 2D matrix $M_u(\boldsymbol{r},t)$ to perform the decomposition. Higher dimensions of SVD do exist but are not required here as the horizontal $x$ and vertical $y$ dimension do not differ when considering axial motion artifacts. SVD is the generalization of the eigendecomposition of a positive semidefinite normal matrix, and can be thought of as decomposing a matrix into a weighted, ordered sum of separable matrices which will become handy when reconstructing the denoised signals:

$$ M_u = U\Sigma V^\star = \sum _ { i } \sigma _ { i } \mathbf { U } _ { i } \otimes \mathbf { V } _ { i } $$

where $U$ contains the spatial eigenvectors, $V$ contains the temporal eigenvectors and $\Sigma$ contains the eigenvalues associated with spatial and temporal eigenvectors. Performing the SVD on an unfolded $M_u(1440\times 1440,512)$ dynamic stack takes around 30 seconds on a workstation computer (Intel i7-7820X CPU, 128Go of DDR4 2666 MHz RAM) and requires 45Go of available RAM using LAPACK routine for SVD computation without computing full matrices. Investigating such decomposition for artifact-free data sets we found that spatial eigenvectors related to motion artifacts have particular and easily identifiable associated temporal eigenvectors. Indeed, when looking at the first temporal eigenvectors, we observed sinusoid-like patterns with increasing frequency. In presence of motion artifacts, temporal eigenvectors appeared with random, high-frequency components that are easy to detect with simple features. Here, the zero crossing rate (the number of time a function crosses $y=0$) is used to detect temporal eigenvectors involved in motion artifacts. In presence of motion artifacts some of the firsts temporal eigenvectors present a high zero crossing rate. In order to detect these outliers we computed the absolute value derivative of the zero crossing rate (D-ZRC) and applied a threshold: if the D-ZRC is higher than three times the D-ZRC standard deviation then the corresponding eigenvalue is set to zero in $\hat{\Sigma}$ and the denoised stack $\hat{M}_u$ is reconstructed as:

$$ \hat{M}_u = \sum _ { i } \hat{\sigma _ { i }} \mathbf { U } _ { i } \otimes \mathbf { V } _ { i }  $$


The denoised stack $\hat{M}_u$ can then be folded back to its original 3D shape $\hat{M}$ and the dynamic computation can be performed. Interestingly, the use of an automatic selection of eigenvectors allow a more reproducible analysis. For example, the SVD can be performed on spatial sub-regions without visible artifacts, something very hard to obtain with manual selection of eigenvectors. This can also improve the filtering procedure in the case of sample spatial deformation or if the computation requires too much RAM. 

### Results

Applying the proposed method on clinical data taken on cancerous lung biopsies we show that it reduces effectively artifacts arising from highly reflective structures motion:

![SVD results 1](../img/svd_results.pdf){: .center-image }
*(a)(d) Original DFFOCT images computed on the raw stack. (b)(e) Denoised images computed with SVD filtering. (c)(f) Sum of the spatial eigenvectors absolute value removed by the SVD filtering.*

## Cumulative sum for deeper imaging


