## Advanced Lane Line Detection Project Writeup
---

## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points

**Advanced Lane Finding Project**

The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

[//]: # "image reference"
[chess_undist]: ./output_images/chess_board_undist.png "chess_undist"
[car_undist]: ./output_images/car_undistort_figure.png "car_undist"
[thresh_result]: output_images/thresh_result.png "thresh_test"
[perspective_trans_result]: ./output_images/per_trans_result.png "perspective_transformation"
[fitting_result]: ./output_images/fitting_result.png "fitting result"
[single_result]: ./output_images/drawn_result.png "drawn result"

[link1]: https://classroom.udacity.com/nanodegrees/nd013/parts/168c60f1-cc92-450a-a91b-e427c326e6a7/modules/5d1efbaa-27d0-4ad5-a67a-48729ccebd9c/lessons/78afdfc4-f0fa-4505-b890-5d8e6319e15c/concepts/a30f45cb-c1c0-482c-8e78-a26604841ec0 "calibration link"

[link2]: https://classroom.udacity.com/nanodegrees/nd013/parts/168c60f1-cc92-450a-a91b-e427c326e6a7/modules/5d1efbaa-27d0-4ad5-a67a-48729ccebd9c/lessons/626f183c-593e-41d7-a828-eda3c6122573/concepts/4dd9f2c2-1722-412f-9a02-eec3de0c2207 "line fitting link"

[link3]: https://classroom.udacity.com/nanodegrees/nd013/parts/168c60f1-cc92-450a-a91b-e427c326e6a7/modules/5d1efbaa-27d0-4ad5-a67a-48729ccebd9c/lessons/626f183c-593e-41d7-a828-eda3c6122573/concepts/1a352727-390e-469d-87ea-c91cd78869d6 "Curvature ratio"

[link4]: https://classroom.udacity.com/nanodegrees/nd013/parts/168c60f1-cc92-450a-a91b-e427c326e6a7/modules/5d1efbaa-27d0-4ad5-a67a-48729ccebd9c/lessons/626f183c-593e-41d7-a828-eda3c6122573/concepts/2f928913-21f6-4611-9055-01744acc344f "Curvature equation"
### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

All codes are in file"./advanced_lane_detection.ipynb" 

### Camera Calibration

1. Set up objective chess board corner coordiantes which represent the location of internal corners. Assuming all chess celles are with 1 unit length and width (referencing video in [link1]). Also set up a list of detected corners from images by using **cv2.findChessboardCorners**

2. Apply **cv2.calibrateCamera** to each image with objectvie and image points to get calibrated camera matrix and distort coefficients

3. Undistort each image using **cv2.undistort** with calibrated camera matrix and distort coefficients

![alt text][chess_undist]

### Pipeline (single images)
#### 1. Provide an example of a distortion-corrected image.
The objective points and image corner points from chess board are used to undistort the car camera images.
The bumps near two sides of front cover are flatten in the undistorted image.
![alt text][car_undist]


#### 2. Threshold Binary image
This step provides a binary image that show lane line pixels from the original image. 

1. Apply sobel magnitude threshold to the image. The magnitude is a weighted sum of sobel x and y. By default, the mignitude is totally contributed by sobel x, because lane lines are usually nearlly vertical.

2. Apply saturation threshold to the image.

3. Select pixels that met one of the above threshold to create a binary image.

4. Select pixels within a region of interest as lane line pixels.
![alt text][thresh_result]

#### 3. Bird's view perspective transform
Set up a trapezoid in the original image and calculate its transformation matrix to a rectangle which it should be when viewed from bird's view. The transformation matrix is calculated by **cv2.getPerspectiveTransform(src, dst)**

When setting up the trapezoid, the height of the trapezoid was tuned until the resulted lane lines are almost parallel to each other.
![alt text][perspective_trans_result]

#### 4. Lane Line pixel extraction and line fitting

The pipeline of extracting lane line pixels is following the process in [link2], which has two major steps: pixel selection, and line fitting.

1. Create a histogram of the "top" half of the image as the "bottom" half is usually noisy.

2. Find the middle x coordinates for left and lane lines by the peak positions in the left and right halves of the histogram.

3. Create sliding windows to scan feasible pixels along the lane lines. The windows should positioned around the center of the lane lines and its height(number) and width influence the scanning precision. The width should be close to the true width of lane line and the height(number) should be enough to follow the curvature if the lane lines are curved.

![alt text][fitting_result]

Tested on the same image as in [link2].

#### 5. Measuring Curvature

1. Translating pixel coordinate into meters, used the same scaling ratio as in  [link3]

ym_per_pix = 30/720 # meters per pixel in y dimension
xm_per_pix = 3.7/700 # meters per pixel in x dimension

and re-calculate the fitting coefficients for each lane line in meters unit.

2. Calculate curvature used the equation in [link4], which is calculated by the fitting coefficients in meters and the bottom y coordinates in the image.

In the final overlay output, the curvature is curvature = (left_cur + right_cur) / 2


#### 6. Draw result

Used **cv2.putText** to write text on the undistorted image and apply a mask on it to show lane area.
![alt text][single_result]
