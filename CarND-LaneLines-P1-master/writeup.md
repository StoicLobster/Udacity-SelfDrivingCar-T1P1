# **Finding Lane Lines on the Road** 

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[RawImage]: ./reflection_images/RawImage.png "Raw Image"
[WY_Select]: ./reflection_images/WY_Select.png "White / Yellow Select"
[Canny]: ./reflection_images/Canny.png "Canny"
[Region]: ./reflection_images/Region.png "Region Select"
[Final]: ./reflection_images/Final.png "Final Result"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 6 steps. The process will be described on this sample image:

![alt text][RawImage]

1. Perform a white / yellow color select on the image. This step will turn the image black except for any white or yellow pixels,
which will be turned pure white [255, 255, 255].

![alt text][WY_Select]

2. Perform a grayscale conversion. This doesnt change the appearance of the image, but only consolidates the 3 channel image
down to 1 channel for use in Canny edge detection.

3. Perform Canny edge detection algorithm.

![alt text][Canny]

4. Perform region select for a trapezoid which encloses roadlines for all sample images.

![alt text][Region]

5. Perform Hough Transform and select / extrapolate appropriate lines. The output lines of the Hough transform are first filtered 
based on slope. Any slope that is "near zero" (horizontal) will be omitted. The "near zero" slope is calculated proportionally to
the aspect ratio of the original image. Sufficiently vertical lines are then separated into right / left lane markers by slope sign.
For a given side, the average line segment slope, center x, and center y is calculated. Average is a weight of line segment length
(to help omit small non-continuous segments). Finally, the lane marker line (defined by average slope, average x, and average y) is
extrapolated to the bottom of the image and to an appropriate vertical postion (near the center of the image). The final lane marker
lines are plotted on top of the original image below:

![alt text][Final]


### 2. Identify potential shortcomings with your current pipeline


1. I notice that the resultant lane lines in the sample videos are fairly shaky / unsteady. This would surely introduce downstream errors in
lateral vehicle control and localization.

2. My pipeline is dependent on the color selection to appropriately find the white / yellow lane markers. If lane markers have a different hue,
are shrouded in a shadow, or are simply missing, the algorithm will fail.

3. The region select portion of this pipeline assumes a fairly static location for the lane markers. For 90 degree turns in the road or downward hills,
the camera would no longer be looking in the correct location for the lane markers.


### 3. Suggest possible improvements to your pipeline

1. The unstable nature of the lane lines might be improved with a temproal fileter. This would improve stability but would sacrifice responsiveness.

2. This algorithm is most appropriate for highways with well defined lane markers, so perhaps knowledge of the vehicle speed / GPS location would aid
in applying this algorithm only when it is expected to find such lane markers (on highways).

3. Probably the hue detection algorithm (to find white / yellow markers) could be improved by trying to understand if there is a shadow on the given pixel.
A gradient computation might be able to detect shadowed areas where a different hue threshold could be used for white / yellow.
