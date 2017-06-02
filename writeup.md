# **Finding Lane Lines on the Road** 

## Writeup 

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps.
1. convert the images to grayscale
2. blur the image using gaussian filter
3. extract edges using canny filter
4. get the edges in the region of interest
5. detect lines using hough transform method


In order to draw a single line on the left and right lanes,
I modified the draw_lines() function by 
1. separating the lines into two groups, one is the right line the other is the left line, using the slope of lines. I set two thresholds and check if the slope of line is between the thresholds, to distinguish whether a line belongs to left or right.
2. fitting a line using `numpy.polyfit` for each group
3. apply a mean filter, using the last 10 results, to smooth the result.

and after that, I apply the roi-mask to the image which lines are drawn.

If you'd like to include images to show how the pipeline works, here is how to include an image: 

![output example][./test_images_output/solidWhiteCurve_output.png]


### 2. Identify potential shortcomings with your current pipeline

1. the current fixed region of interest will not work when the car is changing its lane, or on the crossroad.
2. with the current filtering method, i will see a time-delay, especially when the car turns quickly.
3. when the road is coarse, edge-detection layer will get lots of lines and i cannot extract (highly-)potential lines.
4. the current line fitting method is not robust to outliers.

### 3. Suggest possible improvements to your pipeline

1. roi -> create a elastic roi for every image based on the position of the car, the positions of other surrounding cars, and the map.
2. filtering -> I can predict the lines in the next frame/image using the car's velocity.
3. edge-detection failure -> extract just a color(while/yellow) image, before convert the raw image to grayscale
4. not robust to outliers -> RANSAC should be better

