---
layout: post
title: "Matlab vs Python"
subtitle: "Open source is life"
author: jules_scholler
image: /img/matlab_vs_python.jpg
---

At the beginning of my PhD I switched from Matlab to Python, here I'll explain why and develop the pros and cons of using Python for your scientific research.

## Why switching in the first place?

To be honest I was very happy with Matlab (who would not when you know how much they charge!) but I decided to go full open source for my PhD (it's like being vegan but on-line!) because I think that we should spread our knowledge and projects in order for everyone to be able to use it. In the academic field more and more people are using Python and the more people are using it, the better it gets with more modules and libraries.

## About Python

The first difference between Python and Matlab is setuping your environment. With Matlab it's easy (again, you pay for it!) you just download and install it. With Python there are numerous way to install and use it. For scientific development I would recommend to use an *IDE*. Personally I use **Spyder** because it has numerous features that I like (especially the variable explorer and iPython console). In order to install Python I used Anaconda (which is an open-source distribution designed for scientific computing) which comes with numerous packages and library (including **Spyder**) that are essential when programming for sciences. The most important libraries are:
- Scipy (optimization, linear algebra, integration, interpolation, special functions, FFT, signal processing)
- Numpy (support for large, multi-dimensional arrays and matrices)
- Matplotlib (plotting library)

With this three previous libraries you can have the equivalent of a base Matlab. Then there are a lot of famous libraries (equivalent to Matlab toolboxes), the ones that I often use are:

- Scikit-image (image processing)
- Scikit-learn (machine learning)
- Keras (deep learning)
- Tensorflow (deep learning)
- Trackpy (object tracking)
- PyautoGUI (interaction with OS)

## Pros

- Python is great as a whole programming language (it is its original purpose), there are keywords that do not have equivalent in Matlab (e.g 'in') that are very handy, especially when it comes to handle paths, files and directories.
- It is object oriented, meaning you can define class and methods leading to a clearer code.
- It is completely free an open source, which is very suitable for academia and personal use.
- The overall productivity is, *in fine*, the same as Matlab. It took me about 3 weeks to be fluent in Python and get back my coding productivity.
- The Python community is getting bigger and bigger, leading to more and more library and resources.

## Cons

- Python is hard for starting as the installation process can be complicated for beginners. Indeed it requires to use bash commands in a terminal environment for adding libraries and it do not come complete out of the box as Matlab.
- I also personally think that Matlab figures are prettier by default and you can have more interactions with them. In addition you can save *.fig* and modify them afterwards, which is very cool to share them.
- It can be a problem to use someone else code on another environment (virtual env. is the solution though!)





