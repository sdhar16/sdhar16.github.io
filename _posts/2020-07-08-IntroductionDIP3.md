---
title: Introduction to Digital Image Processing III
---

Applying filters to an image is an everyday task in not only digital image processing but most of
the machine learning applications that deal with images, especially convolution neural networks.
But to first use these techniques for machine learning we must first learn to apply even the most
basic filters to images.
To learn this we will take an example of blurring an image.

## Image Blur
Image blurring is nothing new, we use it in most of the image processing applications. It is
primarily used to reduce noise from an image before some other algorithm is applied on it. Don’t
worry what noise means, we will discuss it later.
The most basic tool for image blurring is the averaging filter. It is as simple as it sounds. We
basically take a pixel in the original image, calculate the average of the pixel values in the KxK
neighborhood of that pixel and put in that value in the pixel of the new image. We call this KxK
neighborhood window or filter with size K.

| ![]({{site.imgpath}}/intro-to-dip/image18.png) | 
| :-----: |
| Mean Flter |

In this example the concerned pixel is 1 in the middle, we take a `3x3` area around that pixel,
calculate the average of these pixel values and put that average in place of 1 in the new image.
But you must also think about what happens to the pixel on the edge or at the corners, how can
we take a KxK window? To solve this we simply pad the image with zeros, so that average can
be calculated for those corner pixels. If the filter size is `KxK` we will need to pad with `K-1` in all
directions, and the new image size would be `(M+2K-2) x (N+2K-2)`.

The code it is fairly easy, try it on your own first.

```python
def average_filter(img, k): # filter size = k
    img2 = np.zeros(img.shape).astype("uint8")
    img = np.pad(img, (k-1, k-1), "constant", constant_values=(0,0))
    for i in range(img2.shape[0]):
        for j in range(img2.shape[1]):
            mean = np.mean(img[i:i+2*k-2,j:j+2*k-2])
            img2[i,j] = mean
    return img2
img = cv2.imread("elon.jpg", 0)
img2 = average_filter(img, 3)
cv2.imwrite("mean_blurred.jpg", img2)
```


| ![]({{site.imgpath}}/intro-to-dip/image19.jpg) |  ![]({{site.imgpath}}/intro-to-dip/image8.jpg) |
| :-----: | :------: |
| Original  | Mean Blur |


As you can see the image is now blurred, try this with other filter sizes and note your
observations. See that the filter is always odd, otherwise you cannot select a central pixel.


## Median Filter
Now you will do something even mode interesting, you will remove noise from an image.
Noise is something that reduces the “quality” of an image, it is undesirable and can occur due to
various reasons, such as defects in the camera, error while transmitting the image, etc.
In image processing every image will have some sort of noise in it. We also try to reduce this
noise so that the image looks “good” to us.

### Salt and pepper
Probably the simplest noise is salt and pepper, and it is exactly what it is named after. Salt is
white in color and pepper is black, so it is the presence of black and white spots on the image,
the same when you mix some salt and pepper together.

| ![]({{site.imgpath}}/intro-to-dip/image20.jpg) | 
| :-----: |
| 30% Salt and Pepper Noise |

The easiest way to remove salt and pepper noise is a median filter. It is similar to the mean filter
above but instead of taking the mean we take the median of the pixel values in the `KxK`
neighborhood.


| ![]({{site.imgpath}}/intro-to-dip/image12.png) | 
| :-----: |
| Median Filter |

| ![]({{site.imgpath}}/intro-to-dip/image20.jpg) |  ![]({{site.imgpath}}/intro-to-dip/image22.jpg) |
| :-----: | :------: |
| Salt and Pepper Noise  | Median Filtered |

Try to add salt and pepper noise to the image first, then try to denoise it.

```python
def salt_pepper(img, prob):
    for i in range(img.shape[0]):
        for j in range(img.shape[1]):
            if(np.random.random()<prob):
                if(np.random.random()<0.5):
                    img[i,j] = 0
                else:
                    img[i,j] = 255
    return img
```

```python
def median_filter(img, k): # filter size = k
    img2 = np.zeros(img.shape).astype("uint8")
    img = np.pad(img, (k-1, k-1), "constant", constant_values=(0,0))
    for i in range(img2.shape[0]):
        for j in range(img2.shape[1]):
            median = np.median(img[i:i+2*k-2,j:j+2*k-2])
            img2[i,j] = median
    return img2
img = cv2.imread("elon.jpg", 0)
noise = salt_pepper(img, 0.3)
cv2.imwrite("noise.jpg", noise)
img2 = median_filter(noise, 3)
cv2.imwrite("median_blurred.jpg", img2)
```


In this code we first add how much salt and pepper noise we need, and also that both salt and
pepper are in equal proportions. Then we denoise the image with a median filter.
The result is pretty satisfying, that even such a noisy can be easily cleaned.