# **Finding Lane Lines on the Road** 

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./test_images_output/white_1.jpg "Pipeline output"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

```python
img = np.copy(img_src)
img = grayscale(img)
img = gaussian_blur(img, kernel_size)
img = canny(img, low_threshold, high_threshold)
img = region_of_interest(img, mask)
img = hough_lines(img, rho, theta, threshold, min_line_len, max_line_gap)
img = weighted_img(img, img_src, α=0.5)
```

My pipeline consists of 5 steps. First, I convert the image to grayscale, then I apply a gaussian blur convolution with a kernel size of 5 pixels to reduce the noise. In order to detect the borders, I apply a Canny filter with `low_threshold=30` and `high_threshold=150`. These parameters are selected to detect lines in conditions of low contrast. Then I mask out all the points that lie off a trapezium-shaped region at the bottom half of the image, before applying the Hough transform in order to detect lines in the region of interest. Then, using a modified version of `draw_lines()`, I calculate the slopes of these lines to classify them as right or left lines. I also filter out lines that are close to horizontal. Then I calculate the averages of these two sets of lines, using the least-squares method, in order to draw two single lines that will be overimposed on the original image.

Example of output image after applying the pipeline.

![Pipeline output][image1]


### 2. Identify potential shortcomings with your current pipeline

There are several shortcomings with the current pipeline. 

First, its parameters are manually fixed and don't adapt to varying conditions (e.g. different weather, lighting conditions, time of the day, type of pavement, color of the painting and shadows on the road).

It only detects straight lines, therefore it has problems following curves and slopes on the road.

### 3. Suggest possible improvements to your pipeline

A possible improvement would be to add the possibility to adapt the pipeline's parameters to the varying conditions, avoiding manual tuning. Ideally, these parameters tuning should be automated.

Another possible improvement would be to use previous frames to predict the current one. One easy way to implement this would be passing the previous frame(s) lines' slopes in order to prevent sudden changes of their magnitudes.

Finally, it would be desirable to detect curved lines (i.e. using least-squares method with degree > 1.)
