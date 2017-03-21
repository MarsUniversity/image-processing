---
title: "Image Basics"
teaching: 30
exercises: 0
questions:
- "What are the questions?"
objectives:
- "What are the objectives?"
keypoints:
- "What are the key points?"
---

The images we see on hard copy, view with our electronic devices, or process 
with our programs are represented and stored in the computer as numeric 
abstractions, approximations of what we see with our eyes in the real world. 
Before we begin to learn how to process images with Python programs, we need 
to spend some time understanding how these abstractions work. 

## Pixels

First it is important to realize that images are stored as rectangular arrays 
of hundreds, thousands, or millions of discrete "picture elements," otherwise 
known as pixels. Each pixel can be thought of as a single point of colored 
light.

For example, consider this image of a maize seedling, with a square area 
designated by a red box:

![Original size image](../fig/01-original.jpg)

Now, if we zoomed in close enough to see the pixels in the red box, we would 
see something like this:

![Enlarged image area](../fig/01-enlarged.jpg)

Note that each circle in the enlarged image area -- each pixel -- is all one 
color, but that each pixel can have a different color from its neighbors. 
Viewed from a distance, these pixels seem to blend together to form the image 
we see. 

## Coordinate system

When we process images, we can access, examine, and / or change the color of 
any pixel we wish. To do this, we need some convention on how to access pixels 
individually; a way to give each one a name or an address of sort. 

The most common manner to do this, and the one we will use in our programs, 
is to assign a modified Cartesian coordinate system to the image. The 
coordinate system we usually see in mathematics has a horizontal x-axis and 
a vertical y-axis, like this:

![Cartesian coordinate system](../fig/01-cartesian.png)

The modified coordinate system used for our images will have only positive 
coordinates, the origin will be in the upper left corner instead of the 
center, and y coordinate values will get larger as they go down instead of 
up, like this:

![Image coordinate system](../fig/01-image-coordinates.png)

This is called a *left-hand coordinate system*. If you hold your left hand 
in front of your face and point your thumb at the floor, your extended index 
finger will correspond to the x-axis while your thumb represents the y-axis.

![Left-hand coordinate system](../fig/01-left-hand-coordinates.png)

Until you have worked with images for a while, the most common mistake that 
you will make with coordinates is to forget that y coordinates get larger 
as they go down instead of up as in a normal Cartesian coordinate system. 

## Color model

Digital images use some color model to create a broad range of colors from 
a small set of primary colors. Although there are several different color 
models that are used for images, the most commonly occurring one is the 
RGB model. 

The RGB model is an *additive* color model, which means that the primary 
colors are mixed together to form other colors. In the RGB model, the 
primary colors are red, green, and blue -- thus the name of the model. 
Each primary color is often called a *channel*. 

Most frequently, the amount of the primary color added is represented as 
an integer in the closed range [0, 255]. Therefore, there are 256 discrete 
amounts of each primary color that can be added to produce another color. 
The value 256 corresponds to the number of bits used to hold the color 
channel value, eight (since 2<sup>8</sup>=256). Since we have three channels, 
this is called 24-bit color depth. 

Any particular color in the RGB model can be expressed by a triplet of 
integers in [0, 255], representing the red, green, and blue channels, 
respectively. A larger number in a channel means that more of that primary 
color is present. 

This image shows some color names, their 24-bit RGB triplet values, and the 
color itself.

![RGB color table](../fig/01-color-table.png)

We will not provide an extensive table, as there are 2<sup>24</sup> = 
16,777,216 possible colors with our additive, 24-bit RGB color model. 

Although 24-bit color depth is common, there are other options. We might have 
8-bit color (3 bits for red and green, but only 2 for blue, providing 8 × 8 × 
4 = 256 colors) or 16-bit color (4 bits for red, green, and blue, plus 4 more 
for transparency, providing 16 × 16 × 16 = 4096 colors), for example. There 
are color depths with more than eight bits per channel, but as the human eye 
can only discern approximately 10 million different colors, these are not 
often used. 

If you are using an older or inexpensive laptop screen or LCD monitor to view 
images, it may only support 18-bit color, capable of displaying 64 × 64 × 64 
= 262,144 colors. 24-bit color images will be converted in some manner to 
18-bit, and thus the color quality you see will not match what is actually in 
the image. 

We can combine our coordinate system with the 24-bit RGB color model to gain a 
conceptual understanding of the images we will be working with. An image is a 
rectangular array of pixels, each with its own coordinate. Each pixel in the 
image is a point of colored light, where the color is specified by a 24-bit RGB 
triplet. Such an image is an example of *raster graphics*. 

## Image formats

Although the images we will manipulate in our programs are conceptualized as 
rectangular arrays of RGB triplets, they are not necessarily created, stored, 
or transmitted in that format. There are several image formats we might 
encounter, and we should know the basics of at least of few of them. Some 
formats we might encounter, and their file extensions, are shown in this table:

| Format                                  | Extension     |
| :-------------------------------------- | :------------ |
| Device-Independent Bitmap (BMP)         | .bmp          |
| Joint Photographic Experts Group (JPEG) | .jpg or .jpeg |
| Tagged Image File Format (TIFF)         | .tiff         |

## BMP

The file format that comes closest to our conceptualization of images is the 
Device-Independent Bitmap, or BMP, file format. BMP files store raster graphics 
images as long sequences of binary-encoded numbers that specify the color of 
each pixel in the image. Since computer files are one-dimensional structures, 
the pixel colors are stored one row at a time. That is, the first row of pixels 
(those with y-coordinate 0) are stored first, followed by the second row (those 
with y-coordinate 1), and so on. Depending on how it was created, a BMP image 
might have 8-bit, 16-bit, or 24-bit  color depth. 

24-bit BMP images have a relatively simple file format, can be viewed and 
loaded across a wide variety of operating systems, and have high quality. 
However, BMP images are not *compressed*, resulting in very large file sizes 
for any useful image resolutions. 

The idea of image compression is important to us for two reasons: first, 
compressed images have smaller file sizes, and are therefore easier to store 
and transmit; and second, compressed images may not have as much detail as 
their uncompressed counterparts, and so out programs may not be able to detect 
some important aspect if we are working with compressed images. Since 
compression is important to us, we should take a brief detour and discuss 
the concept. 

## Image compression

Imagine that we have a fairly large, but very boring image: a 5,000 × 5,000 
image composed of nothing but white pixels. If we used an uncompressed image 
format such as BMP, how much storage would be required for the file? Well, 
there are

5,000 × 5,000 = 25,000,000

pixels, and 24 bits for each pixel, leading to 

25,000,000 × 26 = 600,000,000

bits, or 75,000,000 bytes (71.5MB). That is quite a lot of space for a very 
uninteresting image! (See the following table for the definitions of 
kilobytes, megabytes, etc. The smallest unit of data we can work with is a 
byte, or eight bits.)

| Unit     | Abbreviation | Size       |
| :------- | ------------ | :--------- |
| Kilobyte | KB           | 1024 bytes |
| Megabyte | MB           | 1024 KB    |
| Gigabyte | GB           | 1024 MB    |

Since image files can be very large, various compression schemes exist for 
saving (approximately) the same information while using less space. 
These compression techniques can be categorized as *lossless* or *lossy*. 

## Lossless compression 

In lossless image compression, we apply some algorithm to the image, resulting 
in a file that is significantly smaller than the uncompressed BMP file 
equivalent would be. Then, when we wish to load and view or process the image, 
our program reads the compressed file, and reverses the compression process, 
resulting in an image that is *identical* to the original. Nothing is lost in 
the process -- hence the term "lossless."

The general idea of lossless compression is to somehow detect long patterns 
of bytes in a file that are repeated over and over, and then assign a smaller 
bit pattern to represent the longer sample. Then, the compressed file is made 
up of the smaller patterns, rather than the larger ones, thus reducing the 
number of bytes required to save the file. The compressed file also contains 
a table of the substituted patterns and the originals, so when the file is 
decompressed it can be made identical to the original before compression. 

To provide you with a concrete example, consider the 71.5 MB white BMP image 
discussed above. When put through the zip compression utility on Microsoft 
Windows, the resulting .zip file is only 72 KB in size! That is, the .zip 
version of the image is three orders of magnitude smaller than the original, 
and it can be decompressed into a file that is byte-for-byte the same as the 
original. Since the original is so repetitious -- simply the same color 
triplet repeated 25,000,000 times -- the compression algorithm can 
dramatically reduce the size of the file. 

If you work with .zip or .gz archives, you are dealing with lossless 
compression. 

## Lossy compression

Lossy compression takes the original image and discards some of the detail 
in it, resulting in a smaller file format. The goal is to only throw away 
detail that someone viewing the image would not notice. Many lossy 
compression schemes have adjustable levels of compression, so that the image 
creator can choose the amount of detail that is lost. The more detail that 
is sacrificed, the smaller the image files will be -- but of course, the 
detail and richness of the image will be lower as well. 

This is probably fine for images that are shown on Web pages or printed off 
on 4 × 6 photo paper, but may or may not be fine for scientific work. You 
will have to decide whether the loss of image quality and detail are important 
to your work, versus the space savings afforded by a lossy compression format. 

It is important to understand that once an image is saved in a lossy 
compression format, the lost detail is just that -- lost. I.e., unlike 
lossless formats, given an image saved in a lossy format, there is no way 
to reconstruct the original image in a byte-by-byte manner. 

## JPEG

JPEG images are perhaps the most commonly encountered digital images today. 
JPEG uses lossy compression, and the degree of compression can be tuned to 
your likings. It supports 24-bit color depth, and since the format is so 
widely used, JPEG images can be viewed and manipulated easily on all 
computing platforms.

Referring back to our large image of white pixels, while BMP required 71.5 MB 
to store the image, the same image stored in JPEG format required only 384 KB 
of storage, a two-orders-of-magnitude improvement. 

Here is an example showing how JPEG compression might impact image quality. 
Consider this image of several maize seedlings (scaled down here from 11,339 
× 11,336 pixels in order to fit the display).

![Original image](../fig/01-quality-orig.jpg)

Now, let us zoom in and look at a small section of the original, first in the 
uncompressed format:

![Enlarged, uncompressed](../fig/01-quality-tif.jpg)

Here is the same area of the image, but in JPEG format. We used a fairly 
aggressive compression parameter to make the JPEG, in order to illustrate 
the problems you might encounter with the format.

![Enlarged, compressed](../fig/01-quality-jpg.jpg)

The JPEG image is of clearly inferior quality. It has less color variation 
and noticeable pixelation. Quality differences become even more marked when 
one examines the color histograms for each image. A histogram shows how 
often each color value appears in an image. First, here is the histogram for 
the uncompressed image:

![Uncompressed histogram](../fig/01-quality-tif-histogram.jpeg)

Now, look at the histogram for the compressed image sample:

![Compressed histogram](../fig/01-quality-jpg-histogram.jpeg)

(We we learn how to make histograms such as these later on in the workshop.)
The differences in the color histograms are even more apparent than in the
images themselves; clearly the JPEG is quite different from the
uncompressed version.

If the quality settings for your JPEG images are high (and the compression 
rate therefore relatively low), the images may be of sufficient quality for 
your work. It all depends on how much quality you need, and what restrictions 
you have on image storage space.

## TIFF

TIFF images are popular with publishers, graphics designers, and photographers. 
TIFF images can be uncompressed, or compressed using either lossless or lossy 
compression schemes, depending on the settings used, and so TIFF images seem 
to have the benefits of both the BMP and JPEG formats. The main disadvantage 
of TIFF images (other than the size of images in the uncompressed version of 
the format) is that they are not universally readable by image viewing and 
manipulation software. 