---
layout: post
title: "Tutorial: fluorescence image processing"
subtitle: "Automatic detection and computation of outer segment orientation"
author: jules_scholler
bigimg: /img/outer_segment_orientation/outer_segment.jpg
image: /img/outer_segment_orientation/outer_segment.jpg
show-avatar: false
---

## Aim of the tutorial

From an input fluorescence image of the photoreceptors shown at the top we want to compute the deviation of the outer-segment (red part) with respect to the base (blue part). Also we want to compute the deviation of the green and the blue part.

You can download the data here.


## Step 1: load and display the data

The first step is to load and display the data. The data consist of a 3 dimensional array of shape (1024,1024,4). The first image is not of interest in our application so we will just disregard it. Find a way to display each image in color separately like below.

![Outer segment](../img/outer_segment_orientation/outer_segment_separate.jpg){: .center-image }

```Python
def display_initial_dataset(u):
    """
    Display initial image.
    
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

## Step 3: loop through the detected outer segment
