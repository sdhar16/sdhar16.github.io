---
title: Introduction to Digital Image Processing I
---
We all have seen and used images in our computers and mobile phones, but many of us don’t know how these images work or how they are processed in our computers. In this series we will discuss how images work in computers, work on some very basic applications and show how images are handled while coding.

## What is digital image processing?
Digital image processing simply means processing of digital images using a digital computer. This processing can mean many things such as displaying the image, zooming the image, applying filters to the image, etc. To understand how these processes work we must first understand how images are used in a digital computer.

## What is a digital image?
An image is basically made of different values of points in a 2D plane. These values can be the
colors in the image, or the intensity of light (for grayscale images). The number of such values
can be infinite, but when we use a finite number of values to represent the image, we call them
digital images. These digital images are used in all of our computers.
For simplicity we will talk about the grayscale images for now


| ![Grayscale image]({{site.imgpath}}/intro-to-dip/image4.png) | 
| :-----: |
| Grayscale image |


Each of these values in a digital image have 2 properties:
1. **Location** - where in the 2D plane.
2. **Intensity** - what is the lightness or darkness in that region.

These values are called pixels. You must have heard about them, for eg. when setting the video
quality in Youtube such as 1080p/720p this is called resolution. An image is made up of many
such pixels.

For a grayscale image the digital image is basically a 2D matrix. Where the values in the matrix
represent their intensity.

Now let us see what properties do digital images have:
1. **Resolution**: This is the measure of how many pixels are in the image. If you see an image with resolution 1920x1080, it means that the image is 1920 pixel long and 1080 pixels wide. The total number of pixels would be 20,73,600. But because this is too hard to remember we just say that its resolution is 1920x1080.

| ![]({{site.imgpath}}/intro-to-dip/image6.png) | ![]({{site.imgpath}}/intro-to-dip/image9.jpg) | ![]({{site.imgpath}}/intro-to-dip/image10.jpg) | 
| :-----: | :----: | :-----: |
| Higher resolution | Lower resolution | Lowest resolution | 


2. **Bit-depth**: Since images are stored in computers the value of pixels is measured in bits.
Remember the value in pixels means the measure of how light or dark the area in the
image is. The more values one can use, the better we can see the light and dark parts.
Generally image pixels are represented by 8-bits, i.e. their value goes from 0-255. But if
the pixels are represented by 1-bit. Then it can only represent black and white colors

| ![]({{site.imgpath}}/intro-to-dip/image17.png) | ![]({{site.imgpath}}/intro-to-dip/image7.png) | ![]({{site.imgpath}}/intro-to-dip/image2.png) | ![]({{site.imgpath}}/intro-to-dip/image4.png) |
| :-----: | :----: | :-----: | :-----: |
| 1 Bit | 2 Bit | 3 bit | 8 Bit | 


## OpenCV
Now we come to the interesting part of this section.
Before we begin you should have Python 3.6+ installed on your computer.
openCV - Is an open-source library mainly used for computer vision. In this and the future posts
we will use openCV for processing and applying algorithms to digital images.
The simplest way to install openCV is to use the unofficial openCV builds. Just use pip and type.

```bash
pip install opencv-python
```

This will install the necessary packages

Open a new file and type:

```python
import cv2
```

If it works then the installation is successful otherwise there are many troubleshooting guides on
the internet.


## Reading an image
Our first task is to read a digital image on our computer, and print it.
Take a grayscale image, any grayscale image will do.
The code will be as follows:

```python
import cv2 # this import is openCV library
img = cv2.imread("img.png", 0) # imread will read the image img.jpg and the argument 0 mean read it in grayscale mode
print(img)
```

Note that the pixel values will lie between 0-255, and the image is a matrix.




## Rotate the image
Our first task was trivial, now since we have the image matrix, we can do anything to this image.
For starters let us try to rotate the image.
We can easily rotate the image using matrix transformation.

```python
import cv2
import numpy as np
img = cv2.imread("img.png", 0)
rot_img = np.rot90(img)
cv2.imwrite("rot_img.jpg", rot_img)
```


| ![]({{site.imgpath}}/intro-to-dip/image14.jpg) | 
| :-----: |
| Rotation |


This is just an example of millions of other things we can do with digital images. In the next part we will learn how we can zoom an image and the various techniques that are
used in it.


