# **Finding Lane Lines on the Road** 

## Writeup

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* To create a pipeline that finds lane lines on the road
* Realize the short comings of this technique and suggest possible improvements


[//]: # (Image References)

[image1]: ./examples/actual.png "original"
[image2]: ./examples/yellow_black_filter.png "filtered"
[image3]: ./examples/gray.png "grayscale"
[image4]: ./examples/gaussian_smooth.png "gaussian kernel smoothing"
[image5]: ./examples/canny.png "canny edge detection"
[image6]: ./examples/roi.png "region of interest"
[image7]: ./examples/draw_lines.png "draw lines"
[image8]: ./examples/annotated.png "annotated"

---

### Reflection

### 1. My pipeline - It consists of 9 steps from reading the image to annotating it with lines.

#### 1.1 Read the image

Import necessary packages and read the image from a location

![alt text][image1]

#### 1.2 Filters

Apply filters to retain only the yellow and black pixels. Lower and upper threshold values can be set. This step was not required to test on the images. But the videos, especially the challenge video required this additional feature.

![alt text][image2]

#### 1.3 Grayscale

Use the openCV function to convert the image from RGB to Gray. This step could never be avoided with initial image tests but after adding the filter step (1.2) the visible significance is less.

![alt text][image3]

#### 1.4 Gaussian Smoothing

The Gaussian kernel is an ideal way to smoothen the image. The noise can be played with by altering the kernel size.

![alt text][image4]

#### 1.5 Canny Edge Detection

The edges are detected in this process. We can see the lanes (either yellow or white) connected as dots. The ratio between the lower and upper threshold should be considered while trying the values.

![alt text][image5]

#### 1.6 Region of interest

There are many unwanted and unrelated edges in the entire image. The vertices of a polygan are mapped onto the image to concentrate only on the region of interest. This step cannot be done before the canny edge because the polygon sides would also be detected as edges in the image. The vertices are chosen as a percentage of the size of the image and not as absolute values. This helps while dealing with images of different resolutions like the challenge video.

![alt text][image6]

#### 1.7 Hough Transform

The hough transform gives a set of lines based on the parameters like rho, theta, threshold, min_line_len and max_line_gap using the points(edges) obtained from canny edge detection. These parameters can be played with a lot to get these lines but they are rough and so cannot be used to annotate the lanes directly.

#### 1.8 Draw Lines

This step separates lines from the hough transform based on the slope and the location of the x axis values. Lines with positive slope and x values greater than the mid point will be grouped as right lanes. Similarly for the left lanes. If there are any points existing on either sides, a linear regression is applied and a single degree fitting line is created. 

![alt text][image7]

#### 1.9 Annotate

The thick lines from the draw lines function are used to annotate the original picture. The final image is as below.

![alt text][image8]



### 2. Potential shortcomings with current pipeline

* If the lanes turn sharply or if the curvature is too much the pipeline might not work as the linear fit lines would not be able to match them.
* Couple of white cars at a closer proximity would pass through the filter and affect drawing the lines. This will also block the view of the entire lane.
* The environment might play a crucial role. The shadows in the video showed the difference in effects. A situation in the night or a rainy day would make it a blunder.  



### 3. Suggest possible improvements to your pipeline

* Usage of a better filter to extract required information. May be an HSV with a filter.
* A higher order fit for drawing the lines
* A system that can detect car at a close distance and draw lines despite of this situation. 
