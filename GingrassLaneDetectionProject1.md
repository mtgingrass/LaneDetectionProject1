
# **Finding Lane Lines on the Road** 

## Mark Gingrass
## March 27, 2018

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

In order to detect lanes on the road, I first had to transform the image to grayscale. This will lead to savings in computations; rather than 255 colors to work with, I am down to just eight. 

Next, I apply a Gaussian blur to the grayscaled image. This will essentially take blocks of pixels and convert the entire block to a single color. We do this in order to provide some help to the next step in the data pipeline, the Canny Edge detection. 

With the blurred gray scalled image, the delta of colors, or gradiant changes, are much more distinguishable. Applying the Canny algorithm detects edges and contours. 

To narrow the playing field, so to speak, I ignore parts of the image by assigning a Region of Interest (ROI). I defined the region as a trapazoidal shape roughly outlining the road lanes up to the horizon. 

Finally, to detect straight lines I use the Hough transform algorithm. Choosing the hyper-parameters such as how many pixels in a row count as a line, or how wide the line is, or if the line can have large or small gaps and still be considered a line was timely but worth exploring. 

I know have lots of straight lines; however, the angle of the lines may not match the angle of the lanes I am trying to detect. I take care of this problem using another function, the draw_line function described below.

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by first finding each Hough line's average 'x' and 'y' values as well as the average of the slopes. I then removed outlier slopes by using the median +- 2* std deviation. Next I found the furthest 'x' values for left lane and closest 'x' values for the right lane. Repeat for right lane. Finally, I apply a min 'y' value to take the horizon into account (do not want to draw a lane into the sky). I also chose a max 'y' value of '0' to stretch the lane all the way to bottom of camera view when drawn (remember my ROI is set slightly higher than the bottom - I was preparing for the challenge video ahead of time). 

If you'd like to include images to show how the pipeline works, here is how to include an image: 



### 2. Identify potential shortcomings with your current pipeline


A few shortcummings with the current pipeline are quite obvious. For example, what if I were driving at night? The colors might not make sense. What if I am traveling just over a hill and I can't detect the road at all briefly because my car camera is essentially looking up? 

Some not so obvious shortcumming may include computational efficiecny. I am sure that this code is not ready for "real time" use as it may take too long to render. I don't feel as though I did a great job looking for outliers in this program yet - perhaps when I find more time I will go back to it. Finally, detecting straight lines is very limited. Not only are there curved lines in the real driving world, there are lines that are potentialy missing all together or even bend more than 90 degrees from view. 


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to try out other color schemes besides grayscale. Perhaps a bright/dark blend would be better. 

Another potential improvement could be to average a few frames together to remove the jittery lines. 

Filling in the space between left and right lanes with a color. Perhabs if it were filled with green it would be easier to later on detect objects that might be in the way - after all, we are not just interested in the outer lines, we are interested in the entire road. 
