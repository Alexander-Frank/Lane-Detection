# **Finding Lane Lines on the Road** 

## Writeup


---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./test_images/solidWhiteCurve.jpg "Original"
[image2]: ./test_images/solidWhiteCurve.jpg_with_segment_lines.jpg "Line segments"
[image3]: ./test_images/solidWhiteCurve.jpg_with_solid_lines.jpg "Line segments"

---

### Reflection

### 1. Description of Pipeline

My pipeline consisted of 6 steps. First, I converted the images to grayscale, then I applied Gaussian smoothing. Third, I defined the parameters for Canny edge detection and applied. Next, I prepared the region of interest (four sided polygon). The vertices are calculated based on proportions rather than fixed pixel sizes, which should allow for the same code with different picture/video dimensions (as long as the camera position doesn't change). After that I defined the parameters for Hough transform and ran it on the edge detected image from Canny with the region of interest. Last I added the lines drawn when Hough tranform was run (weighted for opacity).

In order to draw a single line on the left and right lanes, I modified the draw_lines() which is called by the hough_lines(). I started out on the images for development and then moved on to the videos, gradually adapting my code until the code was able to solve all videos, including the challenge.

Modification to draw lines (or, how drwa lines works now):

1. define empty list for collecting slopes and points
2. define two global variables which will initially have [0,0,0,0] each and will update to the last left and right line drawn. Needed in case of no lines detected.
3. Implement try except for case of no lines detected (wil otherwise result in 0 division error) where the exception will draw the previous lines
4. Iterate through lines (output of canny) and calculate slope. Throw out all lines which are "horizontal-ish" (really important for challenge video!). Done by throwing out all lines, which meet: 0.5 > slope > -0.5
5. Determine the sign of the slope and add the slope and points to list for left and right side (if slope < 0 then left, else right)
6. Create functions that will take the y variable and calculate the x variable to draw the final lines. Within these functions (one for left, one for right lane) the average slope is taken and all bs (from y = mx+b) are calculated, then averaged. Lastly, the function is solved for x.
7. Draw final lines (y ponits are known from region of interest, x points are calculted from functions defined) and update the last left and right lane points

The below images display my general approach: 

Original Image:
![alt text][image1]

Started with all lines drawn (this view helped extremely in the challenge video where horizontal lines were screwing up my nice code, that's how I got to incorporating the slope check):
![alt text][image2]

Worked my way to this, which is the final output:
![alt text][image3]


### 2.Potential shortcomings with current pipeline


One potential shortcoming would be what would happen when the road curves more extremely. The region of interest wouldn't necessarily be adequate in curves. 

Another shortcoming could be that my lines are still a bit "jumpy", not perfectly smooth yet.



### 3. Possible improvements

A possible improvement would be to further enhance throwing out anomalies, not only when looking at the slopes initially, but also when comparing the current line drawn to the previous onw. If for ehatever reason the line changed position drastically (e.g. more than x % left or right of last lane).

Another potential improvement could be to incorporate curved lines to more adequately adapt to curves in the road. With this, the region of interes should also be changed to adapt for the ange of the curve.

Another potential improvement could be to implement a moving average over the last x lines drawn to smooth the lines and make them less "jumpy".

Another potential improvement could be to work on some names of functions/variables and small things within the code to make it better.
