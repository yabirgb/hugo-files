+++
title = 'Creating a sepia filter with python'
date = '2019-02-17'
author = 'Yábir García'
tags = ['python']
+++

## Introduction

Last week I wanted to emulate the css effects in python on
images. The filter that caught my attention was the sepia filter.  I
write this because I couldn't find much information about the topic on
the web.

## Basics
 
A quick search on your favourite search engine leads to [this
page](https://www.techrepublic.com/blog/how-do-i/how-do-i-convert-images-to-grayscale-and-sepia-tone-using-c/)
where as mentioned in stackoverflow ([^1]) what you have to do is:
 
	outputRed = (inputRed * .393) + (inputGreen *.769) + (inputBlue * .189)
	outputGreen = (inputRed * .349) + (inputGreen *.686) + (inputBlue * .168)
	outputBlue = (inputRed * .272) + (inputGreen *.534) + (inputBlue * .131)

The result is (Image by Andrea Ranalleta on Unsplash):

<img src="/images/andrea-ranalletta-1368337-unsplash-min.jpg" style="display: float;  align-self:center; margin-left:16.5%; width:33%">
<img src="/images/andrea-ranalletta-1368337-unsplash-min.sepia.jpg" style="display: float;  align-self:center; width:33%">

## Coding this on python

With this information what you have to do is to ignore the alpha layer
and for each pixel calculate a bunch of numbers.

You can use `Pillow` for this and then on your program:

    from PIL import Image

    def sepia(image_path:str)->Image:
        img = Image.open(image_path)
        width, height = img.size

        pixels = img.load() # create the pixel map

        for py in range(height):
         for px in range(width):
             r, g, b = img.getpixel((px, py))

             tr = int(0.393 * r + 0.769 * g + 0.189 * b)
             tg = int(0.349 * r + 0.686 * g + 0.168 * b)
             tb = int(0.272 * r + 0.534 * g + 0.131 * b)

             if tr > 255:
                 tr = 255

             if tg > 255:
                 tg = 255

             if tb > 255:
                 tb = 255

             pixels[px, py] = (tr,tg,tb)

        return img
            
Here I'm assuming that the image is a `jpeg`, but in case of a `png ` is 
similar but taking care of the alpha layer. This is easy, but very **slow**, so ... how can this be faster?

My next search was for applying filters to images and I discovered cv2.
Scrolling fast the docs I wrote this:


    import cv2
    import numpy as np
    from PIL import Image
    
    def sepia_cv(image_path:str)->Image:
        """
        Optimization on the sepia filter using cv2 
        """
        
        image = Image.open(image_path)

        # Load the image as an array so cv knows how to work with it
        img = np.array(image)

        # Apply a transformation where we multiply each pixel rgb 
		# with the matrix for the sepia
		
        filt = cv2.transform( img, np.matrix([[ 0.393, 0.769, 0.189],
                                              [ 0.349, 0.686, 0.168],
                                              [ 0.272, 0.534, 0.131]                                  
        ]) )

        # Check wich entries have a value greather than 255 and set it to 255
        filt[np.where(filt>255)] = 255
            
        # Create an image from the array 
        return Image.fromarray(filt)
            

What is happening here is that the previous sums and products are no 
more than a linear transformation between subspaces of the reals, 
so we can represent this as a matrix. But loading `cv2` and `numpy` 
for this is too much so let's just use numpy.

    def sepia_np(image_path:str)->Image:
        """
        Optimization on the sepia filter using numpy 
        """
        
        image = Image.open(image_path)

        # Load the image as an array so cv knows how to work with it
        img = np.array(image)

        # Apply a transformation where we multiply each pixel 
        # rgb with the matrix transformation for the sepia
        
        lmap = np.matrix([[ 0.393, 0.769, 0.189],
                          [ 0.349, 0.686, 0.168],
                          [ 0.272, 0.534, 0.131]                                  
        ])

        filt = np.array([x * lmap.T for x in img] )
        
        # Check wich entries have a value greather than 255 and set it to 255
        filt[np.where(filt>255)] = 255
            
        # Create an image from the array
        return Image.fromarray(filt.astype('uint8'))

In this solution take care of ([^2])

## Improving the filter

In `CSS` you can apply an scale to the filter. After searching a lot I
found [_this
link_](https://drafts.fxtf.org/filter-effects/#sepiaEquivalent) where
it specifies how to apply the filter using this scale. The thing is
that aside the linear map, you are using a movement. The
transformation matrix is:

        matrix = [[ 0.393 + 0.607 * (1 - k), 0.769 - 0.769 * (1 - k), 0.189 - 0.189 * (1 - k)],
        [ 0.349 - 0.349 * (1 - k), 0.686 + 0.314 * (1 - k), 0.168 - 0.168 * (1 - k)],
        [ 0.272 - 0.349 * (1 - k), 0.534 - 0.534* (1 - k), 0.131 + 0.869 * (1 - k)]]
        
where `k` is the interval [0,1].

Here are some examples:

- Sepia with amount = 0 (original image)

<img src="/images/andrea-ranalletta-1368337-unsplash-min.sepia.0.jpg" style="width:33%">

- Sepia with amount = 0.3

<img src="/images/andrea-ranalletta-1368337-unsplash-min.sepia.0.3.jpg" style="width:33%">

- Sepia with amount = 0.7

<img src="/images/andrea-ranalletta-1368337-unsplash-min.sepia.0.7.jpg" style="width:33%">

- Sepia with amount = 1 (the same filter we created initally)

<img src="/images/andrea-ranalletta-1368337-unsplash-min.sepia.1.jpg" style="width:33%">

## Measuring times

Calling `python -m cProfile test.py` ([^3]) with my Pentium G3258 on the image used in this post (2453x2453 pixels):

        ncalls  tottime  percall  cumtime  percall filename:lineno(function)
    
             1    0.007    0.007    0.293    0.293 test.py:34(sepia_cv)
             1    0.033    0.033    0.499    0.499 test.py:56(sepia_np)
             1    7.857    7.857   16.634   16.634 test.py:7(sepia)

This times don't count the time importing the modules used.

[^1]: [How is a sepia tone created?](https://stackoverflow.com/questions/1061093/how-is-a-sepia-tone-created)
[^2]: [Why we need uint8](https://stackoverflow.com/a/27623335)
[^3]: [cProfile](https://docs.python.org/3/library/profile.html)



