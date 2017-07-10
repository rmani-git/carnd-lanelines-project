# **Finding Lane Lines on the Road** 


---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/white_yellow_image.jpg "White and Yellow lanes"

[image2]: ./examples/canny_detection.jpg "Canny detection"

[image3]: ./examples/canny_gaussian.jpg "Smoothing after Canny detection"

[image4]: ./examples/roi.jpg "Region of Interest"

[image5]: ./examples/hough.jpg "Hough lines"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consits of the following steps:

First the input image is converted to an image consisting only of yellow and white colors corresponding to lane lines
This helps to filter out sharp edges on the road which are not corresponding to lane lines

![alt text][image1]

Next image is converted to grayscale and gaussian smoothing applied, to help with edge detection using Canny edge detetion.

![alt text][image2]

Gaussion smoothing is applied again on the canny image to smooth the edges so as to help with line detection.

![alt text][image3]

Then a region of interest is defined so that only the lane region in front of the car is present in the image. 
The vertices of the polygon comprising of the region of interest are based on the dimension of image size

![alt text][image4]

Hough Transform is then applied to detect all the lines in the image. The draw_lines function is used to filter the specific
lanes lines that we need as follows:

- A static class is defined that is used to store the slope of detected lane line from prior frame. Then the mean of the slope 
  of the lane lines in the current frame and the slope from the prior frame is calculated and used as the current frame lane line.
  If there are no lane line detected in the current frame then the lines from prior frame is used.
  This approach ensures that there are no abrupt changes in the lane lines due to say a particular frame does not have any visible
  lane marking

- The lines from the hough transform are divided between positive and negative slopes for the two lane lines. Vertical lines are 
  ignored and only relevant slopes are considred (between 0.5 and 1.25)

- Then the average slope of left and right lane lines are calculated so as to get one left and one right lane line. Then the mean 
  of this lane lines with the prior frame lane line is calulated and used as the current frame's lane lines. The line is also 
  stored for use in consecutive frame.

- Points are then calulated on the lines found above and then extrapolted for the whole image using the image dimensions. 

![alt text][image5]

Only the lines within our region of interest defined earlier is used to combine with the original image to form the final annotated
image with both lane lines drawn as solid lines


### 2. Identify potential shortcomings with your current pipeline

One potential shortcoming would be since we are using straight lines on lanes with very sharp or zizag turns, the annotation will
not follow the curvature of lanes

The current pipe line can only identify lines as lane marking. However if there are other lane markings used such as reflective dots
or reflective posts then the pipeline need to be modified to handle those cases 


### 3. Suggest possible improvements to your pipeline

Extend to different kinds of lane markings

Can try to improve or optimize performance/throughput
