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
[//]: # "link reference"
[link1]: https://classroom.udacity.com/nanodegrees/nd013/parts/168c60f1-cc92-450a-a91b-e427c326e6a7/modules/5d1efbaa-27d0-4ad5-a67a-48729ccebd9c/lessons/78afdfc4-f0fa-4505-b890-5d8e6319e15c/concepts/a30f45cb-c1c0-482c-8e78-a26604841ec0

[//]: # "image reference"
[distort]: ./camera_cal/calibration2.jpg "distort"
[undist]: ./calibration_output/calibration2_output.jpg "undistort"

### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

### Camera Calibration

1. Set up objective chess board corner coordiantes which represent the location of internal corners. Assuming all chess celles are with 1 unit length and width (referencing video in [link1]). Also set up a list of detected corners from images by using **cv2.findChessboardCorners**

2.Apply **cv2.calibrateCamera** to each image with objectvie and image points to get calibrated camera matrix and distort coefficients

3.Undistort each image using **cv2.undistort** with calibrated camera matrix and distort coefficients

**Original Distorted Image**
![alt text][distort]

**Calibrated Image**
![alt text][undist]