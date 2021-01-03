# **Finding Lane Lines on the Road** 

## Writeup


**Finding Lane Lines on the Road**

The goals of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report

![Test Image Output][./test_images_output/solidWhiteRight.jpg]

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the `draw_lines()` function.

My pipeline consisted of 5 steps. First, I converted the images to grayscale, then I .... 



### 2. Identify potential shortcomings with your current pipeline

One potential shortcoming would be that we are drawing straight lines only. Therefore when the the road turns,
the accuracy of our method decreases dramatically.

Another shortcoming could be that drawn lines are wiggling quite a bit when the algorithm is applied to the video.
This happens because slope and intercept of the line changes change their values very often.

Lastly, algorithm relies on the camera being fixed in one particular location. If we were to move camera to other location,
our code would have to re-callibrated.

### 3. Suggest possible improvements to your pipeline

A possible improvement would be to use some kind of moving average of the slope/intercept to reduce line wiggling.

Another potential improvement could be to approximate the lanes by using a curve instead of a line, maybe something like this:
`y = ax +bx^2 + c`.