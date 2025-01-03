---
layout: post
title: "Color stabilization of D-FFOCT timelapse"
subtitle: "Using histogram matching"
author: jules_scholler
image: /img/color_stabilization.jpg
show-avatar: true
---

In this post I will explain how we post-process timelapses to compensate for flickering color effects. When making timelapse of [dynamic full-field OCT](https://www.jscholler.com/2019-01-28-dffoct/) acquisitions colors can slightly shift. This is due to residual noise in the Fourier Transform of the probed signals (I proposed a method for removing such noise [here](https://www.jscholler.com/pages/svd/) but it is not perfect and requires high-end computers to be done. A tradeoff is to use image processing to stabilize the color rendering of timelapses in post processing.

### Method 1: colors are not supposed to change during the acquisition

If colors are not supposed to change during the whole acquisition then one can stabilize the timelapse by taking the colors on the first image as template and match all the following images to the first. To to that you need to do the following:
1. Transform the first image from RGB colorspace to HSV colorspace.
2. Compute the histogram of the H (hue) channel which correspond to the color information, which becomes the template.
3. Loop through the next images, transform them from RGB to HSV colorspace. Then match their H histogram to the template and transform them back to RGB colorspace.
4. The timelapse should be color stabilized!

Below is an example on a several hours retinal pigment epithelium acquisition (see **stabilize_color** below for the source code).

<center>
<iframe width="560" height="315" src="https://www.youtube.com/embed/lJtFcYRsCEw" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</center>

### Method 1: colors are supposed to change slowly during the acquisition

If colors are supposed to change slowly during the whole acquisition (in our case it could be because we expect some biological phenomenon to change over time) then one can remove the fast color flickering by inducing a lowpass filter on the timelapse color.  To to that you need to loop through all images and for each do:
1. Compute the hue channel for the k following images histogram by averaging the hue channel of the k histogram
2. Match the current image hue to the previously computed histogram

(see **stabilize_running_color** below for the source code).


### Source code

```python
def hist_match(source, template):
    """
    Adjust the pixel values of a grayscale image such that its histogram
    matches that of a target image

    Arguments:
    -----------
        source: np.ndarray
            Image to transform; the histogram is computed over the flattened
            array
        template: np.ndarray
            Template image; can have different dimensions to source
    Returns:
    -----------
        matched: np.ndarray
            The transformed output image
    """

    oldshape = source.shape
    source = source.ravel()
    template = template.ravel()

    # get the set of unique pixel values and their corresponding indices and
    # counts
    s_values, bin_idx, s_counts = np.unique(source, return_inverse=True,
                                            return_counts=True)
    t_values, t_counts = np.unique(template, return_counts=True)

    # take the cumsum of the counts and normalize by the number of pixels to
    # get the empirical cumulative distribution functions for the source and
    # template images (maps pixel value --> quantile)
    s_quantiles = np.cumsum(s_counts).astype(np.float64)
    s_quantiles /= s_quantiles[-1]
    t_quantiles = np.cumsum(t_counts).astype(np.float64)
    t_quantiles /= t_quantiles[-1]

    # interpolate linearly to find the pixel values in the template image
    # that correspond most closely to the quantiles in the source image
    interp_t_values = np.interp(s_quantiles, t_quantiles, t_values)

    return interp_t_values[bin_idx].reshape(oldshape)

def stabilize_color(dffoct):
    """
    Stabilize color for timelapses and zStack of the shape [n_image, n_x, n_y, 3]
    It uses the first image as template for the rest.
    Arguments:
    -----------
        dffoct: np.ndarray
            Timelapse (nt,nx,ny) to stabilize
    Returns:
    -----------
        dffoct: np.ndarray
            Color stabilized timelapse
    """
    tmp = rgb2hsv(dffoct[0])
    template = tmp[:,:,0]
    for i in range(dffoct.shape[0]-1):
        tmp = rgb2hsv(dffoct[i+1])
        tmp[:,:,0] = hist_match(tmp[:,:,0], template)
        tmp = hsv2rgb(tmp)
        tmp = rescale_intensity(tmp, out_range='uint8').astype('uint8')
        dffoct[i+1] = tmp
    return dffoct

def stabilize_running_color(dffoct,k):
    """
    Stabilize color for timelapses and zStack of the shape [n_image, n_x, n_y, 3].
    It uses a sliding window of size k to take into account normal changes in color/frequency.
    Arguments:
    -----------
        dffoct: np.ndarray
            Timelapse (nt,nx,ny) to stabilize
        k: int
            Size of the sliding window.
    Returns:
    -----------
        dffoct: np.ndarray
            Color stabilized timelapse
    """
    n,nx,ny,_ = dffoct.shape
    for i in range(dffoct.shape[0]-k):
        h_template = rgb2hsv(dffoct[i:i+k].reshape((k*nx, ny, 3)))[:,:,0]
        tmp = rgb2hsv(dffoct[i])
        tmp[:,:,0] = hist_match(tmp[:,:,0], h_template)
        tmp = hsv2rgb(tmp)
        tmp = rescale_intensity(tmp, out_range='uint8').astype('uint8')
        dffoct[i] = tmp
    return dffoct
```
