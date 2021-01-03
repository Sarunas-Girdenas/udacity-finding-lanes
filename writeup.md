# **Finding Lane Lines on the Road** 

## Writeup


**Finding Lane Lines on the Road**

The goals of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report

![Test Image Output][./test_images_output/solidWhiteRight.jpg]

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the `draw_lines()` function.

My pipeline consisted of 6 steps as shown below:

```
def lane_finding_pipeline(image):
    """
    Given image and parameters, detect Lanes
    """
    
    # 1 step. Make input picture gray 
    gray_img = grayscale(image)
    
    # 2 step. Apply Gaussian filtering
    gaussian_filtered = gaussian_blur(gray_img, kernel_size)
    
    # 3 step. Apply Canny filter to g 
    canny_edges = canny(gaussian_filtered, low_threshold, high_threshold)
    
    # 4 step. Get lines in the area of interest only
    canny_edges_region = region_of_interest(canny_edges, vertices)
    
    # 5 step. Draw lanes using Hough transform
    hough_lines_drawn = hough_lines(canny_edges_region, rho, theta, hough_threshold, min_line_len, max_line_gap)
    
    # 6 step. Overlay lines with the original image
    image_with_lanes = weighted_img(hough_lines_drawn, image)
    
    return image_with_lanes
```

First step converts coloured image to grayscale. Then we apply Gaussian filter (which removes noise by averaging surounding pixels).
After that, we apply Canny filter to detect edges. Since we are interested only in the _triangle_ area in front of the car, we limit ourselves
to that particular region only in step 4. Then in step 5 we use Hough transform to draw lines. Lastly, step 6 overlies the lines onto original picture.

**Changes to `draw_lines()`**

As suggested, I've changed `draw)_lines()` in the following way:
1. For each pair of points (x1, x2, y1, y2) given by Hough Transform we calculate slope and intercept.
2. Then we make sure that those points are in valid range (less/more than +/- infinity) and not _too_ small.
3. For each positive and negative slopes, we calculate average values.
4. Then we use those average to calculate the _X_ coordinate for the line.
5. Finally, draw the lines using those coordinate (X and Y).

We draw lines for each output from Hough as long as its valid.

In order to reduce _wiggling_, I've added the minimum threshold. In other words, we do not draw the line
if the slope is smaller (in absolute value) than the threshold. It seems that this helped a little bit, but not
sure if it makes a huge difference.

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