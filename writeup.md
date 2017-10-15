# **Finding Lane Lines on the Road** 

## Writeup 

---

### Goals 

The goals of this projected is to build an image pipeline to detect lane lines on the road
using Open CV.


[//]: # (Image References)

[input_image]: ./test_images/solidYellowCurve2.jpg "Input Image"
[output_image]: ./test_images_output/solidYellowCurve2.jpg "Output Image"

---

### Reflection

#### Pipeline 

The pipeline consisted of 6 number of steps as listed below.

* Coverting the image to gray scale
* Apply Gaussian blur (`kernal_size=9`)
* Apply Canny edge detection (`low_threshold=50, high_threshold=150`)
* Apply Mask to focus only in region of interest
* Apply Hough Transform to find lines from edges (`rho=1, theta=pi/180, threshold=20, min_line_len=1, max_line_gap=120`)
* Interpolate/extrapolate/average lines form Hough Transform and draw only two lines representing the left and right lanes on the road.

#### Modifying `draw_lines` funciton

The `draw_lines` function initially drew all the lines given my the hough transform. The following are the steps taken to reduce a set of lines
to two lines, one for the left and one for the right lane.

* split lines into two sets, one for left lane lines and right lane lines. Left lines have a positve slope and Right lanes have a negative slope.
* The slope is compared against an experimented value of 0.5, i.e all the lines having a slope greater than 0.5 and less than -0.5 are considered.
* The average of all the points closer the top of the image is considered as the top of the left and right lanes.
* The slope of the left lines is the average slope of all the lines in the left line set. Similary for the right lines.
* Now having a slope of a line and a point (top point) I find the bottom point that intersects with the bottom of the image (i.e `y = height_of_the_image`)
* This leaves us with two points for the left line and two for the right.

#### Results

*Input image to the pipeline*

![alt text][input_image]

*Output image from the pipeline*

![alt text][output_image]

### Potential shortcomings with current pipeline

* I realize that using Canny edge detection on a gray scale image has side effects when there is no good lighting in the scene.

### Suggest possible improvements to your pipeline

* Use a different color space rather then gray scale might be helpful to detect edges in any kind of lighting. Potentially using HSV color space is a good place to start.
