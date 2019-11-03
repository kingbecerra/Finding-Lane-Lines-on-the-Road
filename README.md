# Finding-Lane-Lines-on-the-Road

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

The project was divided in three main sections:
* Testing the solution/algorithm using one image
* Running the algorithm in all images
* Applying the algorithm to the videos

My main pipeline consisted of six steps which are demostrated in the section "Testing Algorithm Step by Step". The algorithm can be executed in five steps by combining step 1 and 2 but I prefered to do it independently for illustrative purposes:

[//]: # (Image References)

[image1]: ./test_images/solidWhiteRight.jpg

### Step 1. Read in the image

We used image ```test_images/solidWhiteRight.jpg``` for testing purposes.

```
image = mpimg.imread('test_images/solidWhiteRight.jpg')
plt.imshow(image)
```

![alt text][image1]

### Step 2. Grayscale the image

### Step 3. Gaussian Smoothing and Canny Edge Detection

### Step 4. Find Vertices

### Step 5. Hough Transform

### Step. 6 Original image with lane lines

My pipeline consisted of 5 steps. First, I converted the images to grayscale, then I .... 

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by ...

If you'd like to include images to show how the pipeline works, here is how to include an image: 

![alt text][image1]

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

My pipeline consisted of 5 steps. First, I converted the images to grayscale, then I .... 

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by ...

If you'd like to include images to show how the pipeline works, here is how to include an image: 

![alt text][image1]


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when ... 

Another shortcoming could be ...


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to ...

Another potential improvement could be to ...
