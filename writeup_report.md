# **Finding Lane Lines on the Road** 

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./test_images_output/test-solidYellowLeft.jpg.png "Test image"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consists of seven steps. First, I convert the images to grayscale, then I apply gaussian blur. Next there is a canny transformation. The relevant edges will be identified by the mask "region of interest". This "cutted" image is the input for cv2.HoughLinesP. The output of cv2.HoughLinesP will be taken as input for the draw_lines() function (see next section). The result of the draw_lines() function will be applied to the original image by the weighted function.

---

In order to draw a single line on the left and right lanes, I modified the draw_lines() function in this way:

There will be a calculation of slope, intercept and length for all the lines coming out of cv2.HoughLinesP. If the slope is negative that line will be put into the group, that formes the line for left lane. If the slope is positive it will be put into the group, that formes the right lane line. If the line is vertical it will be ignored for further consideration.

For each group the average slope will be calculated. The single slope values are weighted by the length of the corresponding line. The same procedure is implemented for intercept.

After identifying slope and intercept for both lane lines (left & right), the start and end points can be calculated and the lines will be drawn.

### 2. Identify potential shortcomings with your current pipeline

I have identified following potential shortcomings:

- Parameters of cv2.HoughLinesP and "region of interest" are tweaked for the training data (first 2 examples), provided in this example. If the input data changes (see challenge example) in regards of "region of interest" or picture quality the pipeline might fall short.
- The output of this pipeline are two straight lines. Therefore this pipeline will have troubles at sharp turns.
- The pipeline works frame by frame independently. That means there is no possibility to average the drawn lines over multiple frames.

### 3. Suggest possible improvements to your pipeline

An improvement would be to take several frames into account for the lines, that will be drawn. That would provide robustness if the identification for single frames is failes or is coming to a wrong result.

Another improvement would be to change the output from linear lines to lines of higher order. That would give the possibility to follow the lane lines in turns more exactly.
