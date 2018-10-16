# **Finding Lane Lines on the Road** 

[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

[solidWhiteCurve_Step09_final]: ./test_images_output/solidWhiteCurve_Step09_final.jpg "solidWhiteCurve final"
![solidWhiteCurve_Step09_final]

Overview
---

When we drive, we use our eyes to decide where to go.  The lines on the road that show us where the lanes are act as our constant reference for where to steer the vehicle.  Naturally, one of the first things we would like to do in developing a self-driving car is to automatically detect lane lines using an algorithm.

This project detects lane lines in images using Python and OpenCV.  OpenCV means "Open-Source Computer Vision", which is a package that has many useful tools for analyzing images.

The project was completed to meet the specifications in the [project rubric](https://review.udacity.com/#!/rubrics/322/view).


Key files in this repository
---
This repository contains the completed first project of the Udacity Self Driving Car Engineer Nano-degree

* **`P1.ipynb`** *- project requirement*
    * Jupyter notebook containing the code implementing the lane-finding pipeline
    * Requires no additional dependencies than the project template required
* **`writeup.md`** *- project requirement*
    * Project reflection including a brief description of each stage in the pipeline, shortcomings, and possible improvements.
* `test_images_output/*.jpg`
    * Examples of a frame at each stage in the pipeline
* `test_videos_output/*.mp4`
    * Video output from the pipeline