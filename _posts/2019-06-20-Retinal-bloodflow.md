---
layout: post
title: "My retinal blood flow"
subtitle: "Laser doppler holography by Léo Puyo"
author: jules_scholler
image: /img/retinal_bloodflow.png
show-avatar: true
---

Léo Puyo, a third year PhD student working in my team, developed a new technique based on holography called **L**aser **D**oppler **H**olography (LDH) and he is able to produce stunning movies of the retinal blood flow. A popular technique already exists for imaging of the retinal vasculature called **O**ptical **C**oherence **T**omography **A**ngiography (OCTA). With this method, clinicians are able to map the retinal vasculature in 3D. The only drawback being that the temporal resolution is not sufficient to see the heartbeat and that the method is not yet quantitative.

On the contrary, LDH has a very high temporal resolution (the camera is working at $60~kHz$) which enables the visualisation of the blood flow. Léo is working on making it quantitative, i.e. extract absolute flow measurements. An example of the videos LDH can generates is shown below.

![Retinal bloodflow](../img/retinal_bloodflow.gif){: .center-image }
*Laser Doppler Holography showing the blood flow in my retina*

#### Experimental setup

![Holography setup(../img/holography_setup.png){: .center-image }
*Laser Doppler holography optical setup. L1, L2 and L3 are converging lenses. PBS:
Polarizing Beam-Splitter. BS: Beam-Splitter. The light source is a single wavelength laser
diode (SWL-7513-H-P, Newport). The Doppler broadened light backscattered by the retina
and choroid is combined with the reference field and interferograms are recorded on the
CMOS camera (Ametek - Phantom V2511) running at 60 kHz.*

The experimental setup consists of a fiber Mach-Zehnder optical interferometer where the light source used for the experiments is a 45 mW and 785 nm single-mode fiber diode laser, spatially and temporally coherent. The wave backscattered by the retina, which bears a Doppler broadened spectrum, is combined with the reference wave using a non-polarizing beamsplitter cube. The polarization of the reference wave is adjusted with a half-wave plate and a polarizer to optimize fringe contrast. The
interferograms formed in the sensor plane are recorded using a CMOS camera (Ametek - Phantom
V2511) with a $f_S = 60 kHz$ frame rate in a $512 \times 512$ format. The diffracted speckle pattern is numerically propagated to the retinal plane using the angular spectrum propagation. After numerical propagation, the beat frequency spectrum of the reconstructed holograms is analyzed by short-time Fourier transform which naturally removes the interferometric zero-order term. In this approach, it is considered that the cross-beating terms of the holograms carry the Doppler broadening caused by the passage of red blood cells and the data processing consists of measuring the local optical temporal fluctuations. 

#### Signal processing
Power Doppler images are calculated by integration of the DPSD over the frequency interval $[f_1, f_2]$. Power Doppler images quantitatively measure blood flow in arbitrary units that depend on both the local blood volume and blood velocity.



#### References

[1] L. Puyo, M. Paques, M. Fink, J.-A. Sahel, and M. Atlan, "In vivo laser Doppler holography of the human retina," Biomed. Opt. Express 9, 4113-4129 (2018)

[2] Léo Puyo, Michel Paques, Mathias Fink, José-Alain Sahel, and Michael Atlan, "Choroidal vasculature imaging with laser Doppler holography," Biomed. Opt. Express 10, 995-1012 (2019)
