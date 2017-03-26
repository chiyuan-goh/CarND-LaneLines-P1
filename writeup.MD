**Finding Lane Lines on the Road**

Lane lines are essential features for driverless cars and advanced driver assistance systems (ADAS) for 

The goals / steps of this project are the following:
* Make a pipeline utilizing canny edge detection and hough transforms that finds lane lines on the road from camera images.
* Reflect on the work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"
[edges_img]: ./writeup_images/edges.png "Edges"
[mask_img]: ./writeup_images/mask.png "Mask"
[aftermask_img]: ./writeup_images/after_mask.png "After Mask"

---

### Overview of Algorithm

###1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

This project introduces us to a brief overview of using computer vision to detect lane lines on road. The current pipeline consists of the following steps:

1. Convert camera image to grayscale
2. Use gaussian filter to smooth the grayscale image [kernel_size = 9]
3. Run the Canny edge detector on the smoothed iamge to extract the line edges [low threshold = 50, high threshold = 220]
![alt text][edges_img]
4. Constrain/mask the image to only the bottom of the image to avoid as many false positives as possible.
5. Run Hough Transform [Opencv's HoughLinesP] to find lines [max_line_gap=4, min_line_len=6, threshold=15, theta= 1 degree, rho = 1] 
6. To draw only the left and right lane lines, the draw_lines() function is modified such that
	1. convert each line to parameter pairs (rho and theta), like in Hough transform to avoid inifinite values.
	2. sort the pairs by theta.
	3. group consecutive pairs together if rho and theta differences are below some tolerance. This collapses lines that are close together.
	4. Sort the groups according to the group size.
	5. average the rho and theta values in the 1st and 2nd largest group.
	6. Draw the 2 lane lines according to values in step 5.

My pipeline consisted of 5 steps. First, I converted the images to grayscale, then I .... 

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by ...

If you'd like to include images to show how the pipeline works, here is how to include an image: 

![alt text][image1]


### Reflection

Obviously, the current algorithm is somewhat elementary, and the parameters used are tuned/fitted to achieve desired outcomes in the test videos.

There are several potential shortcomings which I foresee.

Firstly,the region of interest mask shown above is a particularly tight constraint. It was setted this way to avoid lines found from the edge of the road. This might not translate well into scenarios where the vehicle/camera is either off-center or orientation not being parallel from/with the lane markings. 

Secondly, the line model of the hough transform is only suitable in straight-line lane markings. In the challenge video, we can actually observe lane lines which are curved, therefore higher polynomial models should be adopted.

Environmental issues might also create false positives for the lanes. For instance, the algorithm can easily mistaken a long truck driving in the next lane as a lane marking instead, and thus the lane acan be mistakenly perceived as a lot wider. Since the current parameters are hand tuned to the output videos, worn-out lane markings and driving under different lighting conditions (e.g shadows) might aalso affect the Canny and Hough transform functions, possibly missing out on false negatives.

###2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when ... 

Another shortcoming could be ...


###3. Suggest possible improvements to your pipeline

A possible improvement would be to ...

Another potential improvement could be to ...
