## **Advanced Lane Finding Project**

The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

[image1]: ./output_images/image1.png "Undistorted"
[image2]: ./output_images/image2.png "Undistorted (pipeline)"
[image3]: ./output_images/image3.png "Original and Sobel on x-axis"
[image4]: ./output_images/image4.png "Sobel on y-axis and Thresholded Magnitude"
[image5]: ./output_images/image5.png "Direction of gradient and Combined thresholds"
[image6]: ./output_images/image6.png "Undistorted and S-threshold"
[image7]: ./output_images/image7.png "Undistorted and Final thresholded"
[image8]: ./output_images/image8.png "Warp calibration"
[image9]: ./output_images/image9.png "Warp test"
[image10]: ./output_images/image10.png "Histogram"
[image11]: ./output_images/image11.png "Sliding windows"
[image12]: ./output_images/image12.png "Fit visual"
[image13]: ./output_images/image13.png "Output"
[video14]: ./project_video.mp4 "Video"

## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points

#### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---

### Writeup / README

#### 1. Writeup / README includes all the rubric points.  [Here](https://github.com/udacity/CarND-Advanced-Lane-Lines/blob/master/writeup_template.md) is a template writeup for this project.  

You're reading it!

### Camera Calibration

#### 1. Compute camera matrix and distortion coefficients.

**Step 1:** Prepare "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. 

Assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time algorithm successfully detects all chessboard corners in a test image.

`imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

**Step 2:** Use the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.

**Step 3:** Apply this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 

![alt text][image1]

### Pipeline (single images)

#### 1. Example of a distortion-corrected image.

![alt text][image2]

#### 2. Methods to create a thresholded binary image.

Pipeline contains combination of color and gradient thresholds to generate a binary image (thresholding steps at cell #5 through #7.

**Step 1:** Apply Sobel on x-axis (calculate directional gradient and apply gradient threshold)

**Step 2:** Apply Sobel on y-axis

**Step 3:** Apply Sobel x and y, compute the magnitude of the gradient and apply a threshold

**Step 4:** Apply Sobel x and y, computes the direction of the gradient and apply a threshold

**Step 5:** Combine the thresholds from steps 1-4

**Step 6:** Threshold color channel (S channel from HLS color space)

**Step 7:** Combine color and gradient thresholds (steps 5-6)

**Results:**

![alt text][image3]

![alt text][image4]

![alt text][image5]

![alt text][image6]

![alt text][image7]

#### 3. Perspective transform.

A perspective transform maps the points in a given image to a different, desired, image points with a new perspective. I use the OpenCV functions getPerspectiveTransform() and warpPerspective() to generate a bird's-eye view of a lane from above, which is useful for calculating the lane curvature.

![alt text][image8]

Example with warped and masked image:

![alt text][image9]

#### 4. Identify lane-line pixels and fit their positions with a polynomial.

Steps to fit lane lines with a 2nd order polynomial:

**Step 1:** Find the starting point for the left and right lines (take a histogram of the bottom half of the masked image)

![alt text][image10]

**Step 2:** Set the width of the windows +/- margin (= 100), set minimum number of pixels found to recenter window (= 50)

**Step 3:** If lines are not detected - step through the windows one by one

![alt text][image11]

**Step 4:** Extract left and right line pixel positions and Fit a second order polynomial to each (using np.polyfit

**Step 5:** Generate x and y values for plotting

#### 5. Calculate the radius of curvature of the lane and the position of the vehicle with respect to center.

Fit the left and right lines with a second order polynomial shown in green:

![alt text][image12]

#### 6. Result plotted back down onto the road such that the lane area is identified clearly.


Here is an example of result on a test image:

![alt text][image13]

---

### Pipeline (video)

Here's a [link to my video result](./project_output.mp4)

Here's [link to my video result on YouTube](https://www.youtube.com/watch?v=AVRK_7IG318)

---

### Discussion

#### Difficulties:

Pipeline becames relatively nonstable in cases of very fast turns and difficult light conditions (experimental way to fix this issue is to use high light sensitive camera with high fps values).

#### What could be improoved:

- Drawing procedure could be improved by adding stabilization features

- More experiments with threshold values and color spaces (and different combination of both) could be performed

- Include additional information on each frame (statistics, etc.)

---

### Another Solution here:
"https://github.com/mziedan/P4_Advanced_Lane_Detection-Another-Solution"
