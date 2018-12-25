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

OCT is an imaging technique used to acquire images in scattering samples with *micrometer resolution* (smaller than a hair!). A scattering sample is a sample that *interacts* with light, in the sens that, during the propagation of light inside it, the light will not travel straight but will be deviated (possibly multiple times). We can highlight this phenomenon with clouds, take a look at the following picture:

![Cloudy mountain](../img/clouds_mountain.jpg){: .center-image }

In this example we can distinguish 3 cases for the propagation of light:

1. **Ballistic** light, this light is not scattered even once. You can clearly see the mountain. In order to image ballistic light you can use a basic camera.
2. **Single scattered** light, this light as been scattered exactly once (on average). It is still possible to see through (because some photons are still not scattered, scattering is a random process!). In order to image in single scattering media it is still possible to use a basic camera and a little bit of post processing can do the trick to improve the image quality.
3. **Multiple scattered** light, this light as been scattered multiple times and it is not possible to see through anymore with conventional imaging method.

This is for this third case that OCT is useful, it can distinguish between multiple scattered light and ballistic light. It can therefore extract only the ballistic light by filtering the multiple scattered light and allow us to image inside scattering media, that is to say, image the mountain behind the clouds. The limit being the amount of collected light that has not been scattered, which decreases with the imaging depth and the scttering power of the sample. There are several techniques that can image inside scattering media, the best famous being medical ultrasound imaging. The main difference between ultrasound and OCT is the resolution, because ultrasound uses mechanical waves with greater wavelength, the resolution is around 1 millimeter, which is $100 \sim 1000$ times higher than OCT.

## How does it work?

OCT can be thought as the optical version of medical ultrasound. In medical ultrasound imaging, an ultrasonic pulse is sent inside the medium, the sound propagates and might be reflected by different structures inside the medium. These reflections are recorded by the ultrasonic probe. For each detected echos the time between the emission and the reception is measured and converted into a distance (assuming constant velocity). By latteraly scanning it is then possible to reconstruct an image of the medium. It would be nice to be able to do the same with light but it is not possible. Indeed, the speed of light is so fast that we can not measure accuratly the time between the emission and the reception. To overcome this problem, the idea is to use a typical property of light: **coherence**. Lets consider that we have two light beam:

$$ S_1 = A_1 cos(wt+\phi_1) $$

$$ S_2 = A_2 cos(wt+\phi_2) $$

If we sum these two waves we can have different results depending on the phase difference $\Delta \phi = \phi_1 - \phi_2$. Two examples are shown on the following figure.

![Interferences with two waves](../img/interference_of_two_waves.png){: .center-image }

On the left you have what we would call a constructive summation and on the left a destructive summation (it is also possible to obtain partial constructive summation). For this to work the two beams need to be coherent, which requires a coherent source of light and to be careful when designing the optical system. Traditional OCT (which I am not using, I will cover that in another post!) uses laser sources. I made an animation to represent how it works, see below. The laser beam is splitted in two, one beam is sent inside the sample and the other one is reflected by a mirror, which we call reference mirror. The idea is then to look at the coherent summation between the two beams, that produces interferences. The trick is that only the light that was reflected inside the sample at a distance (the correct term would be the optical path length) that matches the distance with the mirror can produce interference. So if we shift the mirror then it is possible to acquire a line (called A-Scan) corresponding to the backscaterred light intensity. By moving the sample laterally it is then possible to reconstruct the sample in 2D or 3D.

<center>
<iframe width="560" height="315" src="https://www.youtube.com/embed/yHVU5-zMBNE?rel=0" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</center>

This was the explanation for time domain OCT, there are a lot of variants to increase the acquisition speed but the idea behind it remains the same.

## Limitations of OCT

The first limitation is the acquisition speed. If one wants high lateral resolution then it requires to perform small lateral shift so there is a trade off between the field on view and the resolution. Also, the optical properties of traditional OCT due to its design implies that the maximal lateral resolution (as opposed to its axial resolution) is bad. The low numerical aperture required to penetrate deep enough inside samples to reconstruct signals is limiting the axial resolution. Traditional OCT is therefore perfectly fitted when requiring high axial resolution and medium lateral resolution, that is why it is heavily used by ophtalmologists who want to see the different retina layer which are axialy spaced by only several micrometers.

To overcome these limitations, our team at Institut Langevin developped the full field version of OCT, which I will cover in another post.
