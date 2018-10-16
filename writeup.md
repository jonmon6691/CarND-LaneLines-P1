# **Finding Lane Lines on the Road** 

## Writeup

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on the work in a written report


[//]: # (Image References)

[solidWhiteCurve_Step00_original]: ./test_images_output/solidWhiteCurve_Step00_original.jpg "solidWhiteCurve original"
[solidWhiteCurve_Step01_gray]: ./test_images_output/solidWhiteCurve_Step01_gray.jpg "solidWhiteCurve gray"
[solidWhiteCurve_Step02_blur]: ./test_images_output/solidWhiteCurve_Step02_blur.jpg "solidWhiteCurve blur"
[solidWhiteCurve_Step03_canny]: ./test_images_output/solidWhiteCurve_Step03_canny.jpg "solidWhiteCurve canny"
[solidWhiteCurve_Step04_mask]: ./test_images_output/solidWhiteCurve_Step04_mask.jpg "solidWhiteCurve mask"
[solidWhiteCurve_Step05_masked]: ./test_images_output/solidWhiteCurve_Step05_masked.jpg "solidWhiteCurve masked"
[solidWhiteCurve_Step06_rawhough]: ./test_images_output/solidWhiteCurve_Step06_rawhough.jpg "solidWhiteCurve rawhough"
[solidWhiteCurve_Step07_hough]: ./test_images_output/solidWhiteCurve_Step07_hough.jpg "solidWhiteCurve hough"
[solidWhiteCurve_Step08_masked_hough]: ./test_images_output/solidWhiteCurve_Step08_masked_hough.jpg "solidWhiteCurve masked_hough"
[solidWhiteCurve_Step09_final]: ./test_images_output/solidWhiteCurve_Step09_final.jpg "solidWhiteCurve final"

---

### Reflection

### 1. The pipeline.

|  | Overview of the lane detection pipeline. |
|-------------------|---|
| ![solidWhiteCurve_Step00_original] | The pipeline presented consists of 9 steps that take a frame from the input and detect two lanes in a specific region of interest and output the image with two lines drawn on top that indicate the detected line. An example input frame is given to the left. |
| ![solidWhiteCurve_Step01_gray] | The first step is to convert the image to grayscale. This reduces the complexity by ignoring the color channels and allowing the edge detection algorithm to focuses solely on changes in brightness to detect lines. |
| ![solidWhiteCurve_Step02_blur] | The next step is to soften the image with a Gaussian blur. In this pipeline, the kernel was chosen to be 7. This is essentially a low-pass filter for the image that decreases the number of elements that get detected as an edge by the canny algorithm. |
| ![solidWhiteCurve_Step03_canny] | The Canny algorithm is then applied to the softened image. The parameters used were an upper threshold of 120 and a lower threshold of 50. These values were chosen in a parameter search that sought to minimize edges detected over the whole image, while robustly detecting the lines of the lane markers over all of the given test material.|
| ![solidWhiteCurve_Step04_mask] | The next step was to restrict the region of interest to a trapezoid covering the roadway directly in front of the vehicle. 
| ![solidWhiteCurve_Step05_masked] | This shape was chosen to maximize the impact of the lane on the hough transform step, while minimizing the influence of other edges created by vehicles in front or objects to the sides. The region was also constructed in terms of the image size so that the ROI would be robust against changes in image resolution. |
| ![solidWhiteCurve_Step06_rawhough] | The Hough transform was then applied to the masked edges. This returns the line segments identified in the input image by looking at intersections in Hough space. Each pixel in the input image is a sine wave in Hough space and the location of an intersection of the sine waves of two pixels in Hough space corresponds to the line that passes through those two points in image space. When looking at all of the sine waves corresponding to all the pixels in the masked edges image, regions of Hough space that have more intersections than a given threshold indicate the presence of a line. In this pipeline, Hough space was partitioned into bins with dimension rho=2 and theta=2 degrees and the threshold was 20 intersections. The pixels in that line are then grouped into line segments based on their contiguous length and gap parameters. In this pipeline, the minimum line length was 100 pixels and the maximum gap was 80 pixels. |
| ![solidWhiteCurve_Step07_hough] | The next step was to take the detected lines and determine the two lanes present in the image. This was done by first sorting the lines by their slope. Then splitting the list of lines wherever there was a gap between adjacent values of slope greater than a given threshold. This came from the observation that the lanes tended to generate a number of short line segments along their length  with slopes very close to the same value and that spurious lines due to noise tended to have significantly different slopes. The groups were then sorted according to the total length of line segments they contained and the top two were taken. In practice this did a very good job of picking the lane lines since they tended to generate many lines of moderate length compared to the short lines generated by noise. The start points and end points of the line segments were averaged respectively to generate a mean line segment for the lane which was then extended to the full length of the image. |
| ![solidWhiteCurve_Step08_masked_hough] | The ROI mask is then re-applied. |
| ![solidWhiteCurve_Step09_final] | The final step is to overlay the detected lines image onto the original image. |

### 2. Potential shortcomings with the current pipeline


While this pipeline performs well on the test images and video given, it falls apart quite badly on the challenge video. The biggest reason for this is due to the shadows that appear on the left lane-line and in that region of the video. The boundaries of the shadow on the pavement create dozens of erroneous edges that confound those that indicate the location of the lane.

The lane finding pipeline is a small component of a self driving car system and its output is used in algorithms that actuate the controls of the vehicle. The system would have clear specifications for the interfaces between these components and in this case, clearly describe the way that the lanes should be described for blocks downstream. In this project, the output is a raster image that would be of little use to further processing algorithms and is a major shortcoming of the pipeline as implemented in this project.

A trivial example of an output specification that would make sense for this pipeline is a two slope-intercept pairs that describe the two lines. But this is still a limitation because it conveys no information about the curvature of the lane ahead. Having lane curvature available would allow a control system to predict steering inputs necessary in the future and better manage the momentum of the vehicle.

In this pipeline, the two lanes are separated from other lines in the region of interest based on the assumption that they are the two longest, most well represented groups of lines detected by the canny edge detector. If there was anything in front of the ego vehicle, another car or truck, marking on the lane itself, or debris, it is likely that the edges detected on that object would overwhelm the lines detected on the lane boundary and one of the two lane outputs would snap to the erroneous object.


### 3. Possible improvements to this pipeline

A big improvement could be achieved by utilizing the HSV color space and selecting the lanes based on hue and saturation while ignoring value. This would enable masking of large parts of the image that were not the yellow or white lane lines regardless of changing brightness or shadows. This change could lead to big improvements in performance on the challenge video.

An assumption can be made that the positions of the lines in the video do not change very much from frame to frame. A stability improvement could be realized by incorporating a moving average that calculates the current lane line position using the current frame as well as the location of the lines in recent frames. This would prevent large jumps of the lane lines when noise defeats the algorithm and an incorrect result is given for a single frame. This failure mode is dominant in the current implementation when subjected to the challenge video, and this improvement would address that directly.

### 4. Closing remarks

This was a satisfying challenge and it forced me to focus on improving my tools and my process for iterative development. I was most productive when I was able to code, test, and change quickly. This proved most valuable when tuning parameters for given functions such as Canny edge detection and the Hough transform line finder. I wrote many test jigs to explore the parameter space quickly and evaluate the results efficiently. A vestige of this is the DebugPipeline class seen in the project notebook. This made it very easy to dump test images to disk while the pipeline was executing and gave me visibility while I was developing it. That said, I found the "heuristic" approach prescribed by this project tedious and fundamentally fragile. The techniques used are just that, techniques, and the resulting pipeline was only as good as the sample set of images used during its development.
