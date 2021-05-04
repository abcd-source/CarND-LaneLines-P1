# **Finding Lane Lines on the Road**

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./output.png "Output"

---

### Reflection

### 1. Description of my solution

My pipeline consisted of three major steps:
1) Frst I run a `pre_process` method which takes the original image and generates an image of the hough lines and a vector of hough lines from the image.

2) After the preprocessing, I extract the lane lines by running `extract_lane_lines()` which
loops over all of the lines from the hough lines vector, seperates the left/right lines and
finds the average slope and intercept of the hough lines which satisfy an outlier rejection filter.

This extraction process is done instead of modifying the original `draw_lines()` function as I wanted to leave the helper functions as-is and implement my own filter function.

3) Once I have extracted the line lines, I draw them on the original image by running `post_process` which takes the left, and right lines, and the image to draw upon.

***Note*** that I draw both the single lines representing the lane lines on the image as well as the
underlying hough lines which are used to form them. Additionally, for debugging purposes I have
printed the # of hough lines found and used to generate the left and right lane line, as well
as the line parameters (m) and (b).

![alt text][image1]

### 2. Potential shortcomings

There are many short comings in this approach:
1) This approach finds linear lane lines, when in reality, they should be polynomials of many orders to properly model real world lane lines.

2) This approach fails when the there is low contrast between the paint and the asphalt, since it relies on the canny edge detector.

3) This approach assumes that the car is in the center of the lanes.

4) This approach doesn't account for large changes in pitch of the road or the vehicle, since the region of interest is a fixed section of the camera field of view.

### 3. Possible improvements

1) Fit a polynomial to the set of hough lines instead of a single line

2) Dynamically adjust the thresholds in the filter with some recursion if there are no edges detected, e.g. keep lowering the canny edge thresholds until some lane lines are found.

3) Dynamically adust the region of interest based on finding the horizon line in the image by using a similiar edge detector.
