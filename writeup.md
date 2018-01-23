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

My pipeline consisted of 4 steps. First, I find the edges by applying a canny transform to a gaussian blured image. Then I mask these to the area where I expect the road, a trapezoid in the lower middle of the screen. Then I use the hough transform to calculate candidates for lines.

In order to draw a single line on the left and right lanes, I modified the draw_lines() function to perform a two step process. First the lines are split into those making up the left and right lane. The splitting into left and right lines is done on the slope, i.e. a line going from bottom left to top right is assumed to be the left marking, a line from bottom right to top left is assumed to be on the right. All lines belonging on to one group are then drawn and a linear line is fitted to the resulting point set. This fitted line is then drawn from the bottom of the screen to the point where it reaches a mask. This approach was chosen because it allows relatively testing of higher order fits, e.g. quadratic polynomials to account for curves.

For images please see the [jupyter notebook here](P1.ipynb).


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when an edge which points the direction of one side of lane markings appears on the other side of the video. By averaging all lines, a few mis detections would shift the fitted line towards the center.

Another shortcoming could be missing or jumpy lines when no lines are detected, or only few and rapidly changing lines are detected.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to further tune paramteters. This could be done manually but I felt it would provide no educational or practical value. Another approach would be to auto tune parameters, i.e. have strict default parameters but in case where no lines are detected, increase sensitivity until a certain number of hough lines is found.

Another potential improvement could be to introduce memory into the pipeline. Since a good fit of lines for one frame is a great estimate for the following frame, this information could be used in multiple ways. Frames where no lines are detected could reuse older fits, possibly adapting them by using the change information of multiple past frames. A sort of moving averages could be applied to the fit parameters to reduce the jumpyness of the fit. Finally, the mask for edge detection could reuse information from the fit, e.g. not look in a trapezoidal area which is a third of the image, but just look for each line specifically where the last fit was.
