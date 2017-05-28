# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

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

![alt text][image1]


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when ... 

Another shortcoming could be ...


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to ...

Another potential improvement could be to ...
