# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

I am following a consistant naming convension. For example if I am opening an image as img and I am doing some processing on it say p1 then my output will be img_p1 and on top of that I am processing p2 then output will be img_p1_p2 and so on.

steps of the pipeline are as follows:-
1. reading a color image as image
2. converting this image to grayscale image
3. applying gaussian low pass filtering on the image to smooth out the small spurious edge because edges are high frequency components.
4. applying canny edge detection on the remaining true boundaries after smoothing.
5. making a trapezoidal region so that it contains only ROI (region of interet) which is the region containing two lines that we want to detect.
6. in the RIOed region we have mostly line segments of many lengths and many directions. Now we use hough transform mathod to find the mahematical equation of the **lines** from **line segments**.
7. now we overlay these detected lines on the original image and display it.

##### draw_lines() function changes
I ranamed this function as draw_lines_better().
hough transform gives list of pair of points. as x1,y1 and x2,y2.
these two points (4 numbers) rpresents a line. and we have many such lines as output of hough transform on an image that contains a white line segment on black background.
now convert all these lines into y = mx + c format (with usual meaning of x,y,m,c).
run through all lines on by one, find average of all positive slopes(m) with corresponding average of all y-intercepts(c)
and find average of all negative slopes(m) with corresponding average of all y-intercepts(c).

define a static(global) slope and intercept for one positive and for one negative lines, with initial values set to average that was obtaied in the previous run of the program when we didn't apply any smoothing inter-frame.
left hand line has positive slope and right line has negative slope.
use exponential smoothing along video frame direction.
this is for stabilization of the found lane lines.


### 2. Identify potential shortcomings with your current pipeline

Shortcoming of the methos will come into picture when:-
1. there no marking on the road.
2. curved road
3. not enough contrast for the marking


### 3. Suggest possible improvements to your pipeline

1. use optical flow 
2. use parallex
2. use road and non road part to find the car location