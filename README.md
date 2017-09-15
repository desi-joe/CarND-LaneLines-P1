# **Finding Lane Lines on the Road** 


**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road



[//]: # (Image References)

[image1]: ./test_images_output/grayscale1.jpg "Grayscale"
[image2]: ./test_images_output/canny.jpg "Canny Edge Detection"
[image3]: ./test_images_output/blur.jpg "Blur"
[image4]: ./test_images_output/hough.jpg "Hough Space"
[image5]: ./test_images_output/roi.jpg "ROI"
[image5]: ./test_images_output/solidWhiteRight.jpg "Grayscale"

---

### Reflection

### 1. Pipeline for detecting lane
My pipeline consisted of 6 steps. 

1. Convert image to grayscale
2. Apply canny edge detection
3. Apply Gaussian blur to make the image smooth
4. Select the area of interest 
5. Detect lines in Hough space and draw the lines
6. Combine the lane lines with original image
 

In order to draw a single line on the left and right lanes, I modified the draw_lines() function.
I found lines in the image using cv2.HoughLinesP function. Next, I process each line by finding it's slope. Each line has a start and end an point. I use the start and end points to calculate slope of the line. 

A line going up from left to right has a positive slope and line going down from left to right has a negative slope. Lines with positive slopes are considered for left lane and lines with negative values are considered for right lane. 

Lines are separated in two groups based on whether they have positive or negative values. Also, I use a range to filter out lines with too high or too low slope value. This filtering cuts down noise. 

Since each line is made of two points I end up with a collection of points for left lane and a collection of points for right lane. 
I use np.polyfit to find slope and y-intercept of the line that best fits the points for left and right lane using linear regression.

For the video, I use a Queue to track a finite number of lane lines from previous frames. When processing the current frame, I use the mean of the lane lines from previous frames. I filter out the current lane line as noise if the slope of the current lane line is not with in a range of the mean slope.  

![alt text][image1]
img#1: Gray Scale

![alt text][image2]
img#2: Canny Edge Detection

![alt text][image3]
img#3: Blur

![alt text][image4]
img#4: Applying Region Of Interest (ROI)

![alt text][image5]
img#5: Final image with lane lines

### 2. Identify potential shortcomings with your current pipeline

Due to time constrain, I was not able to work on the Optional Challenge problem. Which means that this solution only works on straight line. 


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to draw lane line when even when road color changes. 

Another potential improvement could be to continue detecting lane lines even when the lane has a curve. 
