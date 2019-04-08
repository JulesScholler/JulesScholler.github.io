---
layout: page
title: "Motion artifact removal and signal enhancement"
subtitle: "Toward in vivo DFFOCT and deeper imaging"
comments: true
---

You can find my preprint on [arXiv](https://arxiv.org/abs/1904.00810).

**D**ynamic **F**ull **F**ield **OCT** (DFFOCT) is very promising for 4D microscopy with diffraction limited resolution and also for eye imaging. In this paper I present two methods, (i) a **S**ingular **V**alue **D**ecomposition (SVD) based filtering method that automatically removes artifact arising from sample axial motion during the acquisition, and (ii) a new operator based on non-stationnarities to compute the dynamic in order to improve the **S**ignal to **N**oise **R**atio (SNR). While the first method is a great step toward in vivo DFFOCT imaging the second method allow deeper imaging in scattering media.

## SVD filtering for motion artifact removal

### Theoretical considerations

One drawback of DFFOCT lies in its main advantage, it has a nanometer sensitivity. This means that the slightest mechanical vibration will cause a temendous signal variation compared to the typical probed fluctuations. Indeed, the static sample reflexion coefficient is typical two order of magnitude higher than the metabolic fluctuation. This means that mechanical vibration can bury the signal of interests under high artifacts. This was is an issue in the lab because the setup is mounted on a sturdy optical bench that filter out most of the vibrations. Problems arise when clinicians try to use such system in very noisy environment and for in vivo imaging when these artifacts need to be filtered out in order to retrieve the metabolic image. In a previous post, we saw that in the FFOCT camera plane, the resulting electric field is the sum of the backscattered light from both the sample and the reflected light from the reference arm:

$$ I(\boldsymbol{r},t) = \eta \frac{I_0}{4} \left(  R(\boldsymbol{r},t) + R_{inc} + R_{ref} + 2\sqrt{R(\boldsymbol{r},t)\alpha}cos \left( \Delta \phi (\boldsymbol{r},t)) \right) \right)$$

where $I(\boldsymbol{r},t)$ is the intensity recorded at position $\boldsymbol{r}=(x,y)$ and time $t$, $\eta$ is the camera quantum efficiency, $I_0$ is the power LED output impinging the interferometer considering a 50/50 beam-splitter, $R_{ref}$ is the reference mirror reflectivity (i.e. the power reflection coefficient), $R(\boldsymbol{r},t)$ is the sample reflectivity (i.e. the power reflection coefficient) at position $\boldsymbol{r}$ and time $t$, $\Delta \phi(\boldsymbol{r},t)$ is the phase difference between the reference and sample back-scattered signals at position $\boldsymbol{r}$ and time $t$, $I_{incoh} = R_{inc}I_0/4$ is the incoherent light back-scattered by the sample on the camera, mainly due to multiple scattering and reflections out of the coherence volume.

We also saw that the processed dynamic signal can then be written:

$$ I_{dyn}(\boldsymbol{r}) =\frac{1}{N} \sum_i SD\left(\frac{\eta I_0}{2}\sqrt{R_s(\boldsymbol{r},t_{[i,i+\tau]}) R_{ref}}cos\left(\Delta \phi_s(\boldsymbol{r},t_{[i,i+\tau]})\right)\right) $$

where $SD$ is the standard deviation operator, $N$ is the total number of sub-windows, $\tau$ is the sub-windows length so that $t_{[i,i+\tau]}$ is the time corresponding to one sub-window, $R_s$ and $\Delta \phi_s$ are respectively the reflectivity and phase of the local scatterers that induce the temporal fluctuations that DFFOCT aims to measure. In the event of the entire sample small displacements, on the order of the depth of field or smaller, the processed signals will be the sum of the actual local fluctuations and the modulation created by the whole sample motion creating a global phase shift. The resulting artifacts are therefore proportional to the sample reflectivity, which is orders of magnitude higher than the reflectivity of the scatterers probed by DFFOCT (e.g. mitochondria and vesicles) leading to strong artifacts on the dynamic image, which mask the signal of interest.

### Proposed algorithm

The first step is to unfold the 3D $M(x,y,t)$ cube of data into a 2D matrix $M_u(\boldsymbol{r},t)$ to perform the decomposition. Higher dimensions of SVD do exist but are not required here as the horizontal $x$ and vertical $y$ dimension do not differ when considering axial motion artifacts. SVD is the generalization of the eigendecomposition of a positive semidefinite normal matrix, and can be thought of as decomposing a matrix into a weighted, ordered sum of separable matrices which will become handy when reconstructing the denoised signals:

$$ M_u = U\Sigma V^\star = \sum _ { i } \sigma _ { i } \mathbf { U } _ { i } \otimes \mathbf { V } _ { i } $$

where $U$ contains the spatial eigenvectors, $V$ contains the temporal eigenvectors and $\Sigma$ contains the eigenvalues associated with spatial and temporal eigenvectors. Performing the SVD on an unfolded $M_u(1440\times 1440,512)$ dynamic stack takes around 30 seconds on a workstation computer (Intel i7-7820X CPU, 128Go of DDR4 2666 MHz RAM) and requires 45Go of available RAM using LAPACK routine for SVD computation without computing full matrices. Investigating such decomposition for artifact-free data sets we found that spatial eigenvectors related to motion artifacts have particular and easily identifiable associated temporal eigenvectors. Indeed, when looking at the first temporal eigenvectors, we observed sinusoid-like patterns with increasing frequency. In presence of motion artifacts, temporal eigenvectors appeared with random, high-frequency components that are easy to detect with simple features. Here, the zero crossing rate (the number of time a function crosses $y=0$) is used to detect temporal eigenvectors involved in motion artifacts. In presence of motion artifacts some of the firsts temporal eigenvectors present a high zero crossing rate. In order to detect these outliers we computed the absolute value derivative of the zero crossing rate (D-ZRC) and applied a threshold: if the D-ZRC is higher than three times the D-ZRC standard deviation then the corresponding eigenvalue is set to zero in $\hat{\Sigma}$ and the denoised stack $\hat{M}_u$ is reconstructed as:

$$ \hat{M}_u = \sum _ { i } \hat{\sigma _ { i }} \mathbf { U } _ { i } \otimes \mathbf { V } _ { i }  $$


The denoised stack $\hat{M}_u$ can then be folded back to its original 3D shape $\hat{M}$ and the dynamic computation can be performed. Interestingly, the use of an automatic selection of eigenvectors allow a more reproducible analysis. For example, the SVD can be performed on spatial sub-regions without visible artifacts, something very hard to obtain with manual selection of eigenvectors. This can also improve the filtering procedure in the case of sample spatial deformation or if the computation requires too much RAM. 

### Results

Applying the proposed method on clinical data taken on cancerous lung biopsies we show that it reduces effectively artifacts arising from highly reflective structures motion:

![SVD results 1](../img/svd_results.pdf){: .center-image }

## Cumulative sum for deeper imaging

