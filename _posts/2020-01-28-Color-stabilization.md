---
layout: post
title: "Color stabilization of D-FFOCT timelapse"
subtitle: "Using histogram matching"
author: jules_scholler
image: /img/color_stabilization.jpg
show-avatar: true
---

### Problem



### Results

<center>
<iframe width="560" height="315" src="https://www.youtube.com/embed/lJtFcYRsCEw" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</center>

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
