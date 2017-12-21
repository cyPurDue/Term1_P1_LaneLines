---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. First convert the image to grayscale. Second, use low and high thresholds with gaussian blur to detect Canny edges. Third, define the region of interest, to filter out unnecessary area. Fourth, with tuned parameters, apply Hough transform to detect lines based on edges. The enhanced 'draw_lines()' function can be considered as part of this step. The last step is to output the results onto the image, with converting BGR to RGB.

The 'draw_lines()' function has been modified. First, it scans all points of lines and seperates left and right by calculating the slope. If the slope < 0, it is left, and otherwise right. Some artificial numbers are used to bound slope ranges. If a slope is either too small or too large, it is ignored. The squre length of each line is calculated, in order to provide a normalized weight of each line. Here I think the longer line could provide more confidence than the shorter lines, with more precise slope value. Therefore, the longer a line is, the greater weight is given to it. The total slope value is then normalized to calculate 2 k values (left & right). Then I scan all points again, by using geometric relation and calculated k values, to calculate the intercept on x axis. Here the weights are provided again to have better result. Then use the x intercept points as starting points, with k values on each side, calculate the end points with a 'bar' value (pre-set to 330) on y-axis. This 'bar' parameter is set according to the region of interest. After got all 4 points calculated, draw 2 lines with thickness of 10 and red color.

If you'd like to see the original line detection without any averaging or extrapolation, uncomment lines 67 - 69 in the helper function and comment out below to the end of draw_lines() function.



### 2. Identify potential shortcomings with your current pipeline

One potential shortcoming would be what would happen when the road line is not ideally straight. My current region of interest is set a bit long to see faraway but if curved dash lines are detected, it would make the slope calculation biased.  

Another shortcoming could be the definition of region of interest. I currently put a trapezoid zone, which includes all regions in between left & right lanes. If due to some reason, other lines are somehow detected after Hough transform, it would be hard to filter them out.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to provide a better extrapolation/averaging method to deal with all detected lines. Currently I am using line's length to provide weights, and I think using other methods may improve the optimizaion of slope calculation. Using narrow range of slope window also might help.

Another potential improvement could be to make the lane detection more consistant. So far in the video, I can still see some slight oscillation of the red lines, but physically, the road lane should be consistant. Therefore, in the video test, if we can pass this frame's result with certain geometric relation to the next frame, it could help make the detection more consistant and reduce error.
