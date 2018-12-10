---
layout: page
title: ""
subtitle: ""
---

## Mapping the LDOS with super-resolution

This work was done during my research internship when I was in my third year at ESPCI. I worked with Dorian Bouchet under the supervision of Ignaccio Izzedin.

You can find our publication on [Arxiv](https://arxiv.org/abs/1809.02778).

### Purpose

Our aim was to measure the LDOS. Wait, what's that? The **L**ocal **D**ensity **O**f **S**tates (LDOS) is a physical quantity. It can be roughly understand as the number of states that can be occupied by a photon. It is space resolved, meaning that the LDOS depends on where you are on a medium, it is a local property that can explain material behavior. Indeed, the LDOS is involved in the way the light interact with matter. The aim of the project was to image the LDOS in 2D (at the interface between two medium) with a super-resolution.

#### Super-resolution

So you may be wondering what this super-resolution is about. In optics, and in any field that uses waves, there is a physical limit called *diffraction* caused by the wave propagation. In practice, it means that if you are far (in optics you are far from something if you are further than an hair thickness) from what you image, then you cannot obtain a better resolution than the *difraction limit*. This has been a struggle for the pasts centuries, mainly in astronomy and microscopy. If you are interested you should read about it on [Wikipedia](https://en.wikipedia.org/wiki/Diffraction). It also holds for the light you may try to send. If you want to focus light, e.g. with a lens, the focus spot cannot be smaller than this diffraction limit. So what we wanted to do is to beat this diffraction limit. You may think, "But you said it was a physical limit, how can you beat physics?". Well, in fact you are right, you cannot beat physics, but you can use physics in a smarter way. So in order to beat the diffraction there is basically two kinds of methods:
- near field
- far field

Remember that the diffraction limit arise because of the **propagation** of light. So if you probe the light before, or shortly after, it propagates, then you are no longer limited by the diffraction. The other set of methods, called *far field* uses some known properties to improve the resolution. 
