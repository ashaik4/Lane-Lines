# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report
*


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. First, I converted the images to grayscale, then I applied the Gaussian blur to the grayed image with kernel size=3. 
In the third step, I used canny edge detector with low_threshold = 50 and high_threshold=150. 
In the fourth step, I needed to determine a region of interest on the image. I decided to use trapezoidal area as my region of interest. I would need to determine the co-ordinates of the trapezoidal image. To choose the area, I mapped the trapezoid height and widths of two parallel sides as follows: 
  - trapezoid_bottom_width = 0.85  # width of bottom edge of trapezoid, expressed as percentage of image width
  - trapezoid_top_width = 0.07  # ditto for top edge of trapezoid
  - trapezoid_height = 0.4  # height of the trapezoid expressed as percentage of image height
Using the above estimates for the region, I determined the vertices of the trapezoid as follows:
 - Vertices: 
             1. [ width x (1-trapezoid-bottom-width)//2, height]
             2. [width x (1- trapezoid_top_width)//2, (height - height * (trapezoid_height))]
             3. [width - (width x (1 - trapezoid_top_width ))//2,  height - height x trapezoid_height]
             4. [width - (width x (1 - trapezoid_base_width))//2, height]
      
 I limit our canny edge filter to the trapezoidal region. Next, I perform the line colorization in this region (mask).
 
 In the fifth step, I run the Hough Transform on the edge detected image. This accurately detected the lane lines. But, we needed a solid and continuous lines on both sides of the road. In order to accomplish this, we needed to modify the draw_lines() function. 
 
The most important and challenging aspect of this project was to write the draw_lines() function. This function takes the image, lines , color and thickness as parameters. Brief description of each argument is given as follows: 
"""
image: Input image
lines: Hough Transformed lines. list of lists consisting of x1,x2,y1,y2 co-ordinates. 
color: |R|GB. Defines the color of the lines to be drawn. 
Thickness: thickness of the lines 
"""
def draw_lines(image, lines, color=[255, 0, 0], thickness=10)

Using the list, lines given to the function as a parameter, we determine the average line properties like, average slope, average inclination . Based on these parameters, we perform the linear regression. This gives us the solid lines around the lane lines. 

Next, I specify the global parameters in a code cell in jupyter notebook. It consists of parameters like low_threshold, high_threshold, kernels, region of interest specifications and Hough Transform variables. 

Finally, I write a function annotate_image() function which transforms the image step by step using above mentioned functions and returns the final image. 

This function is also applied to the video frames.



![alt text][image1]



### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when the lane lines curve. My algorithm causes the line detection to flicker.

Another shortcoming could be to misidentify the lane lines when there is a lot of traffic on the road. 


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to ...

Another potential improvement could be to ...
