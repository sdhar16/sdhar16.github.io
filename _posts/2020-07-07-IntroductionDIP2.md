---
title: Introduction to Digital Image Processing II
---

You must have zoomed in on images, either in your phones or in your computers. But have any
of you wondered how zooming works. Because images are matrices and when we zoom in
them we should be seeing the pixel values, and not the image.

| ![]({{site.imgpath}}/intro-to-dip/image11.png) | 
| :-----: |
| Zoom lens |

Zooming in or magnification is done though a technique called interpolation. Interpolation means
to fill in the values that should be in between. This might sound complicated but it is quite easy.
An image is basically a matrix, it has some values which represent the intensity. To magnify the
image from say a 100x100 resolution to 150x150 resolution we somehow have to “stretch” the
image. But that is we need to fill 150x150 pixels using 100x100 pixels.
This means that we will have many points whose pixel value we do not know. The technique
used to fill those values is called interpolation. We essentially want to “guess” those values.

## Interpolation
To understand interpolation we will take a small example with a 2x2 matrix and try to “stretch” it
into a 4x4 matrix. That is a 2x zoom for the image

### Nearest Neighbour
The most basic technique used in interpolation is Nearest Neighbor. In which we fill the empty
values using the values which are nearest to the pixel in the original image.

| ![]({{site.imgpath}}/intro-to-dip/image1.png) | 
| :-----: |
| Nearest Neighbour |

As you can see we are simply copying the pixels to who are nearest to the pixel in the original
image.
To understand this mathematically, if you have a scale factor = `c`. And your original image is `M x
N` .
Then the new image will be `c*M X c*N`.

Let the original image be represented by matrix A, and the new image by matrix B.
And the pixels values in the zoomed image will be:
```math
B[i][j] = A[i/c][j/c]
```
Remember the `i/c` and `j/c` need to be integers as they represent the index.

## Coding in Python
The code for this is very easy. Try it on your own first.

```python
img = cv2.imread("img.png", 0)
scale = 2 # Scaling factor, 2x zoom
# Create a empty image whose size should be that of the zoomed image
zoom = np.zeros((scale*img.shape[0], scale*img.shape[1])).astype("uint8")
# Nearest Neighbour interpolation
for i in range(zoom.shape[0]):
 for j in range(zoom.shape[1]):
 zoom[i,j] = img[i//scale,j//scale]
# Save the image
cv2.imwrite("zoomed.png", zoom)
```

| ![]({{site.imgpath}}/intro-to-dip/image13.png) |  ![]({{site.imgpath}}/intro-to-dip/image4.png) |
| :-----: | :------: |
| Nearest Neighbour | Original |


Exercise
Here is exercise for you, try to use the above code or whichever code you used and scale the
image using different scale factors in x and y directions. For example take the x-axis scaling
factor to be 2 and y-axis to be 3.
The image will look like this:


| ![]({{site.imgpath}}/intro-to-dip/image16.png) | 
| :-----: |
| Different axis scaling |

You might not be impressed with the result and think that the images are blurry. This is because
this is the simplest technique for interpolation. We will look at a more advanced technique called
Linear Interpolation.

## Bilinear interpolation
Bilinear interpolation is another technique for interpolation.
Here we consider a 2x2 closest neighbourhood of pixels around the unknown pixel in the
zoomed image, then we take the weighted average of these pixels to fill in the pixel. This will
result in a smoother image.

| ![]({{site.imgpath}}/intro-to-dip/image3.png) | 
| :-----: |
| Bilinear |

You can refer to [https://theailearner.com/2018/12/29/image-processing-bilinear-interpolation/] as
it explains it in more detail.
We will use the openCV inbuilt function to demonstrate bilinear interpolation:

```python
import cv2
import numpy as np
img = cv2.imread("img.png", 0)
bilinear_img = cv2.resize(img,None, fx = 2, fy = 2, interpolation =
cv2.INTER_LINEAR)
cv2.imwrite("bilinear.png", bilinear_img)
```


| ![]({{site.imgpath}}/intro-to-dip/image6.png) | 
| :-----: |
| Bilinear Interpolation |

## Bicubic Interpolation
Another technique that is most commonly used is bicubic interpolation, it uses the 16 nearest
pixels to fill the unknown pixel value. For more in depth knowledge refer to
[https://theailearner.com/2018/12/29/image-processing-bicubic-interpolation/]


| ![]({{site.imgpath}}/intro-to-dip/image15.png) | 
| :-----: |
| Bicubic |


Try using openCV’s resize for bilinear interpolation.

If you look closely bicubic will be the best among these for this particular image. But it is not
always the case. Sometimes even nearest neighbour interpolation will be the best technique for
image interpolation.
In the next post we will see how we apply filters to an image.


