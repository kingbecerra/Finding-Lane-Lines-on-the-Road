# Finding-Lane-Lines-on-the-Road

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

The project was divided in three main sections:
* Testing the solution/algorithm using one image
* Running the algorithm in all images
* Applying the algorithm to the videos

My main pipeline consisted of six steps which are demostrated in the section "Testing Algorithm Step by Step". The algorithm can be executed in five steps by combining step 1 and 2 but I prefered to do it independently for illustrative purposes:

[//]: # (Image References)

[image1]: ./test_images/solidWhiteRight.jpg
[image2]: ./test_algorithm/grayscale_image.png
[image3]: ./test_algorithm/gaussian_and_canny.png
[image4]: ./test_algorithm/vertices.png
[image5]: ./test_algorithm/hough_transform.png
[image6]: ./test_algorithm/original_with_lanes.png

---
## Testing the ALgorithm

---
### Step 1. Read in the image
We used image ```test_images/solidWhiteRight.jpg``` for testing purposes.

```
image = mpimg.imread('test_images/solidWhiteRight.jpg')
plt.imshow(image)
```

![alt text][image1]

### Step 2. Grayscale the image
Convert the image to grayscale before applying a Gaussian blut and Cannt Edge Detection.

```
gray = grayscale(image)
plt.imshow(gray, cmap='gray')
```

![alt text][image2]

### Step 3. Gaussian Smoothing and Canny Edge Detection
A kernel of size 5 was ussed for the Gaussian blur. For the Canny Edge Detection algorithm 50 y 150 were used for low and high threshold parameters.

```
# Gaussian smoothing with a defined kernel size
kernel_size = 5
blur_gray = gaussian_blur(gray, kernel_size)

# Canny Edge Detection Alghorithm
low_threshold = 50
high_threshold = 150
edges = canny(blur_gray, low_threshold,high_threshold)
plt.imshow(edges, cmap='Greys_r')
```

![alt text][image3]

### Step 4. Find Vertices
A quadrilatera shape was used to find the vertices and isolate the region of interest.

```
imshape = image.shape
vertices = np.array([[(0,imshape[0]),(450, 320), (490, 320), (imshape[1],imshape[0])]], dtype=np.int32)
masked_edges = region_of_interest(edges, vertices)
plt.imshow(masked_edges, cmap='Greys_r')
```

![alt text][image4]

### Step 5. Hough Transform
******************* TDB

```
# Hough transform parameters
rho = 2 
theta = np.pi/180 
threshold = 30    
min_line_len = 15 
max_line_gap = 20 

# Find lines
line_image = hough_lines(masked_edges, rho, theta, threshold, min_line_len, max_line_gap)

# Create a "color" binary image to combine with line image
color_edges = np.dstack((edges, edges, edges)) 

# Draw the lines on the edge image
lines_edges = cv2.addWeighted(color_edges, 0.8, line_image, 1, 0) 
plt.imshow(lines_edges)
```

![alt text][image5]

### Step. 6 Original image with lane lines
The lanes lines are sumperimposed over the original image.

```
lines_edges = weighted_img(line_image, image, 0.6)
plt.imshow(lines_edges)
```

![alt text][image6]

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
