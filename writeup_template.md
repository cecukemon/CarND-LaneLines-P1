#**Finding Lane Lines on the Road** 

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./lane_images/lanes_solidWhiteCurve.jpg "img1"
[image2]: ./lane_images/lanes_solidWhiteRight.jpg "img2"
[image3]: ./lane_images/lanes_solidYellowCurve.jpg "img3"
[image4]: ./lane_images/lanes_solidYellowCurve2.jpg "img4"
[image5]: ./lane_images/lanes_solidYellowLeft.jpg "img5"
[image6]: ./lane_images/lanes_whiteCarLaneSwitch.jpg "img6"

---

### Reflection

###1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of the following steps:
- convert the image to grayscale
- apply gaussian blur to remove noise
- apply the Canny edge detection function
- define a 3 point polygon mask and apply it to the image. That way, only the relevant part of the image (the lanes) will be processed further, which makes line detection much easier
- apply Hough transform to find the line segments in the image, using the modified draw_lines2() function to connect and extend the line segments
- apply the polygon mask again to keep only the relevant parts of the extrapolated lines

draw_lines2() function has been modified the following way:
- iterate over the line segments detected by the Hough transform
- sort them into left (positive slope) and right (negative slope) line segments
- use the OpenCV fitLine function to fit a line through all the points of the left and right line segments
- use the OpenCV line function to draw the line on the image

Result:

![alt text][image1]
![alt text][image2]
![alt text][image3]
![alt text][image4]
![alt text][image5]
![alt text][image6]

###2. Identify potential shortcomings with your current pipeline

Using both Hough transform and fitLine seems inefficient.

There is some line jumping visible in the videos.

###3. Suggest possible improvements to your pipeline

The polygon mask could be better refined, with 4 points and better fit.

For a better fit of the extrapolated line, clean up the line segments returned by the Hough transform by throwing out the line segment with the smallest and biggest slope. This would remove line segments that are too divergent / possibly in error, and lead to a better fitted line.

The initialisation and usage of the numpy array in draw_line2 is very inefficient in terms of memory allocation, this would be an issue in production code. Need to figure out the size of the array before defining it.

Try out a different way of line extrapolation. One idea: take the slope average (left or right), take a random point (left or right), fit a line with that slope through that point. This could be faster than fitLine through all the points.

