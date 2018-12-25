---
layout: post
title: "Optical Coherence Tomography"
subtitle: "Layperson's explanation"
image: /img/oct-eye.jpg
bigimg: /img/oct-eye.jpg
show-avatar: false
---

My research focuses on using Optical Coherence Tomography (OCT) to image biological samples. In this post I am going to explain the principle and limitations of traditional OCT.

## What is the purpose of OCT?

OCT is an imaging technique used to acquire images in scattering samples with micrometer resolution. A scattering sample is a sample that interacts with light. We say that it scatters the light, meaning that, during the propagation of light inside the sample, the light will not travel straight but will be deviated (possibly multiple times). There is a famous example to highlight this phenomenon, take a look at the following picture:

![Cloudy mountain](../img/clouds_mountain.jpg){: .center-image }

we can distinguish 3 cases for the propagation of light:

1. Ballistic light, this light is not scattered even once. You can clearly see the mountain. In order to image ballistic light you can use a basic camera.
2. Single scattered light, this light as been scattered exactly once (on average). It is still possible to see through (because some photons are still not scattered, scattering is a random process!). In order to image in single scattering media it is still possible to use a basic camera and a little bit of post processing can do the trick to improve the image quality.
3. Multiple scattered light, this light as been scattered multiple times and it is not possible to see through anymore with conventional imaging method.

This is for this third case that OCT is useful, it can distinguish between multiple scattered light and ballistic light. It can therefore extract only the ballistic light by filtering the multiple scattered light and allow us to image inside scattering media. The limit being the amount of light you collect that has not been scattered, which decreases with the imaging depth. There are several techniques to image inside scattering media, the best famous being mediacl ultrasound imaging. The main difference between ultrasound and OCT is the resolution, because ultrasound uses mechanical waves with greater wavelength, the resolution is around 1 millimeter, that is to say 1000 times higher than OCT (which has a micrometer resolution).

## How does it work?

OCT can be thought as the optical version of medical ultrasound. In ultrasound imaging, an ultrasonic pulse is sent inside the medium, the sound propagates and might be reflected by different structures inside the medium. These reflections are recorded by the ultrasonic probe. For each detected echos the time between the emission and the reception is measured and converted into a distance (assuming constant velocity). By latteraly scanning it is then possible to reconstruct an image of the medium. It would be nice to be able to do the same with light but it is impossible. The speed of light is so fast that we can not measure accuratly the time between the emission and the reception. The idea is then to use a typical property of light: coherence. Lets consider that we have two light beam:

$$ S_1 = A_1 cos(wt+\phi_1) $$

$$ S_2 = A_2 cos(wt+\phi_2) $$

If we sum these two waves we can have different results depending on the phase difference $\Delta \phi = \phi_1 - \phi_2$. Two examples are shown on the following figure.

![Interferences with two waves](../img/interference_of_two_waves.png){: .center-image }

On the left you have what we would call a constructive summation and on the left a destructive summation (it is also possible to obtain partial constructive summation). For this to work the two beams need to be coherent, which requires a coherent source of light and to be careful when designing the optical system. Traditional OCT (which I am not using, I will cover that in another post!) uses laser sources. I made an animation to represent how it works, see below. The laser beam is splitted in two, one beam is sent inside the sample and the other one is reflected by a mirror, which we call reference mirror. The idea is then to look at the coherent summation between the two beams, that produces interferences. The trick is that only the light that was reflected inside the sample at a distance (the correct term would be the optical path length) that matches the distance with the mirror can produce interference. So if we shift the mirror then it is possible to acquire a line (called A-Scan) corresponding to the backscaterred light intensity. By moving the sample laterally it is then possible to reconstruct the sample in 2D or 3D.

<center>
<iframe width="560" height="315" src="https://www.youtube.com/embed/yHVU5-zMBNE?rel=0" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</center>

This was the principle of time domain OCT, there are a lot of variants to increase the acquisition speed but the idea remaines the same.
