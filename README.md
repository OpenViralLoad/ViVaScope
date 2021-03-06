# ViVaScope

## Method 1: Region-Based Counting

**Steps**

1. Capture image, store on embedded device.
2. Grayscale then thresholded using Otsu algorithm (to return binary image).
3. Row by row, analyze each pixel. If unlabeled "region" found, perform DFS on said region, marking each pixel as that specific region label.
4. Count number of regions.

**Air Bubble Reduction**

1. Row by row, pixel by pixel, keep track of the current region. If the algorithm enters a supposed microwell region, leaves it, and then reenters that same region, then there is a hole in that well. Count the number of occurences and subtract from overall well count.

## Method 2: Neural Network

**Steps**

1. Capture image, store on embedded device.
2. User remarks upon the number of wells existing on the chip that was imaged.
3. Software generates an array `K` of blank regions, of length = # wells.
4. Device uses neural networks to identify wells, fibers, dust, etc. and their locations.
5. For wells identified, extract sample pixel of that well region, place within a slot in `K`
6. Do this for the top (# wells) likely well-like objects in the image taken.
7. Perform Otsu and easy counting.


## Problems with Region-Based

1. On control chips (totally empty), OTSU binarizes completely different. Potential solution: analyze range of all found pixel intensities. If range is small, likely no wells glowing. Seems a bit rickety.

2. Wells connected due to fluorescence.

3. Needs cropped chip, or segmentation method.

## Advantages with CNNs

1. No need to crop image. Superclassifier for chips.
2. Identifies wells specifically, only, whereas regions pick up any change in pixel intensity.
3. Could avoid OTSU binarization which tends to be the culprit when wells are lost or specks are counted.
 
