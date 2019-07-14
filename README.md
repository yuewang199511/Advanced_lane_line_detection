## Advanced Lane Finding
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)
![Lanes Image](./examples/example_output.jpg)



---

The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

The program of this project is mainy constructed on:

1. OpenCV

2. Numpy

These technics are employed in this project:

1. Camera calibration and image undistortion

2. Line detection by sobel computation and HSL channel filtering

3. Perspective transformation

4. Line fitting

The images for camera calibration are stored in the folder called `camera_cal`.  The images in `test_images` are for testing your pipeline on single frames. 

The output from each stage of my pipeline are in the folder called `output_images`. The video called `project_video.mp4` is the video my pipeline finally work on.  

A description of the technichs, the pipeline, and the discussion of this project is written in `writeup.md`
