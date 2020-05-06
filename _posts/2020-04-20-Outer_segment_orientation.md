---
layout: post
title: "Tutorial: fluorescence image processing"
subtitle: "Automatic detection and computation of outer segment orientation"
author: jules_scholler
bigimg: /img/outer_segment_orientation/outer_segment.jpg
image: /img/outer_segment_orientation/outer_segment.jpg
show-avatar: false
---

The fluorescence images and idea of this tutorial was proposed by Dr. Anna Verschueren:

*"This image shows macaque photoreceptors (cones) observed by immunofluorescence. To be able to distinguish the cell's different compartiment, fluorescent antibodies of different colors are used to specifically stain the outer segment (red), the inner segment (bleue) and their junction (green). One interesting hypothesis could be validated by observing the deformation of the outer segment compared to the inner segment orientation."*

## Aim of the tutorial

From an input fluorescence image of the photoreceptors (available here and shown at the top) we want to compute the deviation of each outer-segment (red part) with respect to their inner-segment (blue part).

For this tutorial you need the following libraries (if you installed Python with Anaconda you should have everything except Scikit-image):

- Numpy
- Scikit-image
- Matplotlib
- Scipy

You are free to choose your IDE but I recommend to use Spyder rather than Jupyter Notebook as it allows you to have a variable explorer to monitor your workspace.


## Step 1: load and display the data

The first step is to load and display the data. The data consists of a 3 dimensional array of shape $(1024,1024,4)$. The first image is not of interest in our application so we will just disregard it. Each $(1024,1024)$ array are representing fluorescent image in ``uint32``, the first being the blue, the second the green and the third the red. First, code a function to display each image in color separately like below.

![Outer segment](../img/outer_segment_orientation/outer_segment_separate.jpg){: .center-image }

In order to help you, I wrote two functions upon which you can draw inspiration:

```python
def gray2color(u,channel):
    """
    Compute color image from intensity in fluorescence in a given channel.
    Arguments:
    -----------
        u: np.ndarray
            Input fluorescence image (2D).
        channel: int
            Channel to code the image in (0: Red, 1: Green, 2: Blue).
    Returns:
    -----------
        u_color: np.ndarray
            The computed output image in color.
    """
    u_color = np.dstack((
        rescale_intensity(u if channel==0 else np.zeros_like(u), out_range='float'),
        rescale_intensity(u if channel==1 else np.zeros_like(u), out_range='float'),
        rescale_intensity(u if channel==2 else np.zeros_like(u), out_range='float'),
        ))
    return u_color
    
def display_initial_dataset(u):
    """
    Display initial image.
    Arguments:
    -----------
        u: np.ndarray
            Initial hyperstack given by Anna.
    """
    R = u[:,:,3]
    G = u [:,:,2]
    B = u[:,:,1]
    u_rgb = np.dstack((
        rescale_intensity(R, out_range='float'),
        rescale_intensity(G, out_range='float'),
        rescale_intensity(B, out_range='float')
        ))
    R_rgb = gray2color(R,0)
    G_rgb = gray2color(G,1)
    B_rgb = gray2color(B,2)
    fig, ax = plt.subplots(1,4,sharex=True,sharey=True)
    ax[0].imshow(u_rgb)
    ax[0].set_title('Original color image')
    ax[1].imshow(R_rgb)
    ax[1].set_title('Red channel')
    ax[2].imshow(G_rgb)
    ax[2].set_title('Green channel')
    ax[3].imshow(B_rgb)
    ax[3].set_title('Blue channel')
    return (ax, fig)
``` 

## Step 2: Automatically locate and segment outer segment

![Outer segment](../img/outer_segment_orientation/segmentation_results.JPG){: .center-image }

For this try to follow the following steps:

1. Threshold the red channel using one of the threshold function from Scikit-image.
2. Use morphology operators to improve the mask (fill holes and remove small objects)
3. Use the ``label`` and ``regionprops`` functions to identify each outer segment.


## Step 3: loop through the detected outer segment

At this step you should have each outer segment in a list with several properties computed by ``regionprops``. The goal is now to loop through the outer segment and ask the user to manually select some points using ``plt.ginput``. In order to help the user, you can then draw the different vectors that describe the direction of each part:

![Orientation example](../img/outer_segment_orientation/orientation_example.JPG){: .center-image }

Then, compute the angle between each pairs of vectors. To do that we need to do a little bit of geometry. Given two points $a$ and $b$ with coordinates:

$$a=\binom{x_1}{y_1} \ \ b=\binom{x_2}{y_2}$$

The vector $\vec{ab}$ can then be written:

$$\vec{ab}=\binom{x_2-x_1}{y_2-y_1}$$

Computing the dot product between $\vec{ab}$ and $\vec{u_x}$ we have:

$$\vec{ab}.\vec{u_x} = ||\vec{ab}|| \times ||\vec{u_x}|| \times cos(\vec{ab},\vec{u_x})$$

Let's call $\alpha$ the angle between $\vec{ab}$ and $\vec{u_x}$, we can compute it with the following equation:

$$\alpha = arccos\left( \frac{x_2-x_1}{\sqrt{(x_2-x_1)^2+(y_2-y_1)^2}} \right)$$

With this create a function ``get_angle`` which compute the angle between a vector defined by two given points and the abscissa unitary vector. To achieve this you will need to use the arccos function ``np.arccos`` and the square root function ``np.sqrt``.

## Solutions

The complete implementation will be given at the end of the tutorial.

```Python

import numpy as np
import matplotlib.pyplot as plt
from skimage.io import imread
from skimage.exposure import rescale_intensity
from skimage.filters import threshold_triangle, try_all_threshold
from skimage.morphology import remove_small_objects, remove_small_holes, skeletonize, closing, selem
from skimage.measure import label, regionprops
from skimage.color import label2rgb
from scipy import ndimage

# Parameters
path = 'SUM1.tif'
plt.ion()

# Load data
u = imread(path)
R = u[:,:,3]
G = u [:,:,2]
B = u[:,:,1]
u_rgb = np.dstack((
    rescale_intensity(R, out_range='float'),
    rescale_intensity(G, out_range='float'),
    rescale_intensity(B, out_range='float')
    ))

# Display initial data
fig_ini, ax_ini = display_initial_dataset(u)

# Segment red channel
fig, ax = plt.subplots(1,6,sharex=True,sharey=True)
R = rescale_intensity(R, out_range='float')
ax[0].imshow(gray2color(R,0))
ax[0].set_title('Red channel')
t = threshold_triangle(R) # First step: threshold red channel
R_mask = R.copy()
R_mask[R_mask<t] = 0
R_mask[R_mask>0] = 1
ax[1].imshow(R_mask, cmap='gray')
ax[1].set_title('Threshold red channel')
R_mask = remove_small_objects(R_mask.astype(np.bool), min_size=1000) # Remove noise detection
ax[2].imshow(R_mask, cmap='gray')
ax[2].set_title('Remove small objects')
R_mask = remove_small_holes(R_mask, area_threshold=20)  # Remove holes in detection
ax[3].imshow(R_mask, cmap='gray')
ax[3].set_title('Remove small holes')
R_mask = closing(R_mask, selem=selem.disk(3)) # Dilate to homogeneize the shapes
ax[4].imshow(R_mask, cmap='gray')
ax[4].set_title('Dilate mask')
R_cells = get_cells_from_bool(R_mask, plot=True)

figr, axr = plt.subplots(1,1)
axr.set_title('Angle deviation between red and blue')
axr.set_xlabel('Distance from blue [pixel]')
axr.set_ylabel('Angle deviation [rad.]')
axr.set_yticks([-np.pi, -np.pi/2, 0, np.pi/2, np.pi])
axr.set_yticklabels(['$-\pi$','$-\pi/2$','0','$\pi/2$','$\pi$'])
axr.set_ylim([-np.pi, np.pi])
angle_blue_green_tot = []
angle_blue_red_tot = []
for cell in R_cells[0:3]:
    # Initialize figure and display segmented shape +50 pixels in color
    fig, ax = plt.subplots(1,2,figsize=(35,18))
    bbox = cell['bbox']
    x1 = np.max((bbox[0]-50,0))
    x2 = np.min((bbox[2]+50,u_rgb.shape[0]))
    y1 = np.max((bbox[1]-50,0))
    y2 = np.min((bbox[3]+50,u_rgb.shape[1]))
    RGB_cell = u_rgb[x1:x2,y1:y2,:]
    B_ini = gray2color(B[x1:x2,y1:y2],2)
    G_ini = gray2color(G[x1:x2,y1:y2],1)
    R_ini = gray2color(R[x1:x2,y1:y2],0)
    R_cell = cell['image']
    ax[0].imshow(RGB_cell, cmap='gray')
    ax[0].set_title('RGB image')
    
    # Display blue signal and ask user to locate both extremities
    ax[1].imshow(B_ini, cmap='gray')
    ax[1].set_title('Blue channel')
    plt.show()
    plt.pause(0.1)
    coords_blue = plt.ginput(2)
    a = coords_blue[0]
    b = coords_blue[1]
    angle_blue = get_angle(a,b)
    ax[0].plot([a[0],b[0]],[a[1],b[1]],'b--',linewidth=4)
    
    # Display green signal and ask user to locate both extremities
    ax[1].imshow(G_ini, cmap='gray')
    ax[1].set_title('Green channel')
    plt.show()
    plt.pause(0.1)
    coords_green = plt.ginput(2)
    a = coords_green[0]
    b = coords_green[1]
    angle_green = get_angle(a,b)
    angle_blue_green_tot.append(angle_blue-angle_green)
    ax[0].plot([a[0],b[0]],[a[1],b[1]],'g--',linewidth=4)
    
    # Display red signal and ask user to follow red shape
    ax[1].imshow(R_ini, cmap='gray')
    ax[1].set_title('Red channel')
    plt.show()
    plt.pause(0.1)
    coords_red = plt.ginput(-1)
    angle_red = []
    dist_red = []
    for i in range(len(coords_red)-1):
        a = coords_red[i]
        b = coords_red[i+1]
        angle_red.append(angle_blue-get_angle(a,b))
        dist_red.append(0 if i==0 else dist_red[i-1] + np.sqrt((coords_red[i][0]-coords_red[i-1][0])**2+(coords_red[i][1]-coords_red[i-1][1])**2))
        ax[0].plot([a[0],b[0]],[a[1],b[1]],'r--',linewidth=4)
    plt.show()
    plt.pause(0.1)
    axr.plot(dist_red,angle_red)
    angle_blue_red_tot.append({'angles': angle_red, 'distances': dist_red})
    plt.show()
    plt.pause(0.1)
    plt.close(fig)
    ```
