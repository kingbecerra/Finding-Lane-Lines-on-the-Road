# Finding-Lane-Lines-on-the-Road

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

[image7]: ./test_images_output/solidYellowLeft.jpg
[image8]: ./test_images_output/solidYellowCurve.jpg
[image9]: ./test_images_output/solidWhiteRight.jpg
[image10]: ./test_images_output/solidYellowCurve2.jpg
[image11]: ./test_images_output/solidWhiteCurve.jpg
[image12]: ./test_images_output/whiteCarLaneSwitch.jpg


## Testing Algorithm Step by Step


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
A quadrilateral shape was used to find the vertices and isolate the region of interest.

```
imshape = image.shape
vertices = np.array([[(0,imshape[0]),(450, 320), (490, 320), (imshape[1],imshape[0])]], dtype=np.int32)
masked_edges = region_of_interest(edges, vertices)
plt.imshow(masked_edges, cmap='Greys_r')
```

![alt text][image4]

### Step 5. Hough Transform
To ```draw_lines``` function in the helper functions was modified to find the left and right lines. Basically for each line the slope was found, and based on the value of the slope they are categorized as left or right line. Lines with small slopes (almost horizontal) aren't taken in consideration. A first degree polynomial is found using the points of the left and right lanes, and then superimposed on the region of interest to be properly displayed on the image.

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


## Applying the Solution to all Images

Two functions were created to facilitate the process of applying the algorithm to all images. These functions includes all the calls to the needed helper functions and automate the process:

```
def process_image(image):
    # NOTE: The output you return should be a color image (3 channel) for processing video below
    # TODO: put your pipeline here,
    # you should return the final output (image where lines are drawn on lanes)
    
    # Grayscale the image
    gray = grayscale(image)
    # plt.imshow(gray, cmap='gray')
    
    # Gaussian smoothing with a defined kernel size
    kernel_size = 5
    blur_gray = gaussian_blur(gray, kernel_size)

    # Canny Edge Detection Alghorithm
    low_threshold = 50
    high_threshold = 150
    edges = canny(blur_gray, low_threshold,high_threshold)
    # plt.imshow(edges, cmap='Greys_r')
    
    # Find vertices
    imshape = image.shape
    vertices = np.array([[(0,imshape[0]),(450, 320), (490, 320), (imshape[1],imshape[0])]], dtype=np.int32)
    masked_edges = region_of_interest(edges, vertices)
        
    # Hough transform
    """
    rho = distance resolution in pixels of the Hough grid
    theta = angular resolution in radians of the Hough grid
    threshold = minimum number of votes (intersections in Hough grid cell)
    min_line_len = minimum number of pixels making up a line
    max_line_gap = maximum gap in pixels between connectable line segments
    """
    rho = 2 
    theta = np.pi/180 
    threshold = 30    
    min_line_len = 15 
    max_line_gap = 20 
    line_image = hough_lines(masked_edges, rho, theta, threshold, min_line_len, max_line_gap)
    result = weighted_img(line_image, image, 0.6)
    
    return result

def process_and_save_image(image_name):
    image = mpimg.imread('test_images/' + image_name)
    image_with_lanes = process_image(image)
    mpimg.imsave('test_images_output/' + image_name, image_with_lanes)
    
    return image_with_lanes
```

### Image 1 - solidYellowLeft.jpg
```
image_with_lanes = process_and_save_image('solidYellowLeft.jpg')
plt.imshow(image_with_lanes)
```

![alt text][image7]


### Image 2 - solidYellowCurve.jpg
```
image_with_lanes = process_and_save_image('solidYellowCurve.jpg')
plt.imshow(image_with_lanes)
```

![alt text][image8]


### Image 3 - solidWhiteRight.jpg
```
image_with_lanes = process_and_save_image('solidWhiteRight.jpg')
plt.imshow(image_with_lanes)
```

![alt text][image9]


### Image 4 - solidYellowCurve2.jpg
```
image_with_lanes = process_and_save_image('solidYellowCurve2.jpg')
plt.imshow(image_with_lanes)
```

![alt text][image10]


### Image 5 - solidWhiteCurve.jpg
```
image_with_lanes = process_and_save_image('solidWhiteCurve.jpg')
plt.imshow(image_with_lanes)
```

![alt text][image11]


### Image 6 -whiteCarLaneSwitch.jpg
```
image_with_lanes = process_and_save_image('whiteCarLaneSwitch.jpg')
plt.imshow(image_with_lanes)
```

![alt text][image12]


## Testing on Videos
The same process_image function was applied to each frame of the two provides videos. The results are in the ```test_video_output``` directory:

### Video 1 - solidWhiteRight.mp4
```
video_output = 'test_videos_output/solid_white_right.mp4'
clip = VideoFileClip("test_videos/solidWhiteRight.mp4")
video_clip = clip.fl_image(process_image) #NOTE: this function expects color images!!
%time video_clip.write_videofile(video_output, audio=False)
```

### Video 2 - solidYellowLeft.mp4
```
video_output = 'test_videos_output/solid_yellow_left.mp4'
clip = VideoFileClip("test_videos/solidYellowLeft.mp4")
video_clip = clip.fl_image(process_image) #NOTE: this function expects color images!!
%time video_clip.write_videofile(video_output, audio=False)
```

# AQUI ME QUEDE

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
