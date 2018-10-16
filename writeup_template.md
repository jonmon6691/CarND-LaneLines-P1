# **Finding Lane Lines on the Road** 

## Writeup

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"
[solidWhiteCurve_Step00_original]: ./test_images_output/solidWhiteCurve_Step00_original.jpg "solidWhiteCurve original"
[solidWhiteCurve_Step01_gray]: ./test_images_output/solidWhiteCurve_Step01_gray.jpg "solidWhiteCurve gray"
[solidWhiteCurve_Step02_blur]: ./test_images_output/solidWhiteCurve_Step02_blur.jpg "solidWhiteCurve blur"
[solidWhiteCurve_Step03_canny]: ./test_images_output/solidWhiteCurve_Step03_canny.jpg "solidWhiteCurve canny"
[solidWhiteCurve_Step04_mask]: ./test_images_output/solidWhiteCurve_Step04_mask.jpg "solidWhiteCurve mask"
[solidWhiteCurve_Step05_masked]: ./test_images_output/solidWhiteCurve_Step05_masked.jpg "solidWhiteCurve masked"
[solidWhiteCurve_Step06_hough]: ./test_images_output/solidWhiteCurve_Step06_hough.jpg "solidWhiteCurve hough"
[solidWhiteCurve_Step07_masked_hough]: ./test_images_output/solidWhiteCurve_Step07_masked_hough.jpg "solidWhiteCurve masked_hough"
[solidWhiteCurve_Step08_final]: ./test_images_output/solidWhiteCurve_Step08_final.jpg "solidWhiteCurve final"
[solidWhiteRight_Step00_original]: ./test_images_output/solidWhiteRight_Step00_original.jpg "solidWhiteRight original"
[solidWhiteRight_Step01_gray]: ./test_images_output/solidWhiteRight_Step01_gray.jpg "solidWhiteRight gray"
[solidWhiteRight_Step02_blur]: ./test_images_output/solidWhiteRight_Step02_blur.jpg "solidWhiteRight blur"
[solidWhiteRight_Step03_canny]: ./test_images_output/solidWhiteRight_Step03_canny.jpg "solidWhiteRight canny"
[solidWhiteRight_Step04_mask]: ./test_images_output/solidWhiteRight_Step04_mask.jpg "solidWhiteRight mask"
[solidWhiteRight_Step05_masked]: ./test_images_output/solidWhiteRight_Step05_masked.jpg "solidWhiteRight masked"
[solidWhiteRight_Step06_hough]: ./test_images_output/solidWhiteRight_Step06_hough.jpg "solidWhiteRight hough"
[solidWhiteRight_Step07_masked_hough]: ./test_images_output/solidWhiteRight_Step07_masked_hough.jpg "solidWhiteRight masked_hough"
[solidWhiteRight_Step08_final]: ./test_images_output/solidWhiteRight_Step08_final.jpg "solidWhiteRight final"
[solidYellowCurve2_Step00_original]: ./test_images_output/solidYellowCurve2_Step00_original.jpg "solidYellowCurve2 original"
[solidYellowCurve2_Step01_gray]: ./test_images_output/solidYellowCurve2_Step01_gray.jpg "solidYellowCurve2 gray"
[solidYellowCurve2_Step02_blur]: ./test_images_output/solidYellowCurve2_Step02_blur.jpg "solidYellowCurve2 blur"
[solidYellowCurve2_Step03_canny]: ./test_images_output/solidYellowCurve2_Step03_canny.jpg "solidYellowCurve2 canny"
[solidYellowCurve2_Step04_mask]: ./test_images_output/solidYellowCurve2_Step04_mask.jpg "solidYellowCurve2 mask"
[solidYellowCurve2_Step05_masked]: ./test_images_output/solidYellowCurve2_Step05_masked.jpg "solidYellowCurve2 masked"
[solidYellowCurve2_Step06_hough]: ./test_images_output/solidYellowCurve2_Step06_hough.jpg "solidYellowCurve2 hough"
[solidYellowCurve2_Step07_masked_hough]: ./test_images_output/solidYellowCurve2_Step07_masked_hough.jpg "solidYellowCurve2 masked_hough"
[solidYellowCurve2_Step08_final]: ./test_images_output/solidYellowCurve2_Step08_final.jpg "solidYellowCurve2 final"
[solidYellowCurve_Step00_original]: ./test_images_output/solidYellowCurve_Step00_original.jpg "solidYellowCurve original"
[solidYellowCurve_Step01_gray]: ./test_images_output/solidYellowCurve_Step01_gray.jpg "solidYellowCurve gray"
[solidYellowCurve_Step02_blur]: ./test_images_output/solidYellowCurve_Step02_blur.jpg "solidYellowCurve blur"
[solidYellowCurve_Step03_canny]: ./test_images_output/solidYellowCurve_Step03_canny.jpg "solidYellowCurve canny"
[solidYellowCurve_Step04_mask]: ./test_images_output/solidYellowCurve_Step04_mask.jpg "solidYellowCurve mask"
[solidYellowCurve_Step05_masked]: ./test_images_output/solidYellowCurve_Step05_masked.jpg "solidYellowCurve masked"
[solidYellowCurve_Step06_hough]: ./test_images_output/solidYellowCurve_Step06_hough.jpg "solidYellowCurve hough"
[solidYellowCurve_Step07_masked_hough]: ./test_images_output/solidYellowCurve_Step07_masked_hough.jpg "solidYellowCurve masked_hough"
[solidYellowCurve_Step08_final]: ./test_images_output/solidYellowCurve_Step08_final.jpg "solidYellowCurve final"
[solidYellowLeft_Step00_original]: ./test_images_output/solidYellowLeft_Step00_original.jpg "solidYellowLeft original"
[solidYellowLeft_Step01_gray]: ./test_images_output/solidYellowLeft_Step01_gray.jpg "solidYellowLeft gray"
[solidYellowLeft_Step02_blur]: ./test_images_output/solidYellowLeft_Step02_blur.jpg "solidYellowLeft blur"
[solidYellowLeft_Step03_canny]: ./test_images_output/solidYellowLeft_Step03_canny.jpg "solidYellowLeft canny"
[solidYellowLeft_Step04_mask]: ./test_images_output/solidYellowLeft_Step04_mask.jpg "solidYellowLeft mask"
[solidYellowLeft_Step05_masked]: ./test_images_output/solidYellowLeft_Step05_masked.jpg "solidYellowLeft masked"
[solidYellowLeft_Step06_hough]: ./test_images_output/solidYellowLeft_Step06_hough.jpg "solidYellowLeft hough"
[solidYellowLeft_Step07_masked_hough]: ./test_images_output/solidYellowLeft_Step07_masked_hough.jpg "solidYellowLeft masked_hough"
[solidYellowLeft_Step08_final]: ./test_images_output/solidYellowLeft_Step08_final.jpg "solidYellowLeft final"
[whiteCarLaneSwitch_Step00_original]: ./test_images_output/whiteCarLaneSwitch_Step00_original.jpg "whiteCarLaneSwitch original"
[whiteCarLaneSwitch_Step01_gray]: ./test_images_output/whiteCarLaneSwitch_Step01_gray.jpg "whiteCarLaneSwitch gray"
[whiteCarLaneSwitch_Step02_blur]: ./test_images_output/whiteCarLaneSwitch_Step02_blur.jpg "whiteCarLaneSwitch blur"
[whiteCarLaneSwitch_Step03_canny]: ./test_images_output/whiteCarLaneSwitch_Step03_canny.jpg "whiteCarLaneSwitch canny"
[whiteCarLaneSwitch_Step04_mask]: ./test_images_output/whiteCarLaneSwitch_Step04_mask.jpg "whiteCarLaneSwitch mask"
[whiteCarLaneSwitch_Step05_masked]: ./test_images_output/whiteCarLaneSwitch_Step05_masked.jpg "whiteCarLaneSwitch masked"
[whiteCarLaneSwitch_Step06_hough]: ./test_images_output/whiteCarLaneSwitch_Step06_hough.jpg "whiteCarLaneSwitch hough"
[whiteCarLaneSwitch_Step07_masked_hough]: ./test_images_output/whiteCarLaneSwitch_Step07_masked_hough.jpg "whiteCarLaneSwitch masked_hough"
[whiteCarLaneSwitch_Step08_final]: ./test_images_output/whiteCarLaneSwitch_Step08_final.jpg "whiteCarLaneSwitch final"

---

### Reflection

### 1. The pipeline. As part of the description, explain how you modified the draw_lines() function.

The pipeline presented consists of 7 steps that take a frame from the input and detect two lanes in a specific region of intrest and output the image with two lines drawn on top that indicate the detected line.
| Overview of the lane detection pipeline. ||
|-------------------|---|
| ![solidWhiteCurve_Step00_original] |The pipeline presented consists of 7 steps that take a frame from the input and detect two lanes in a specific region of intrest and output the image with two lines drawn on top that indicate the detected line. An example input frame is given to the left.|
| ![solidWhiteCurve_Step01_gray] | The first step is to convert the image to grayscale. This reduces the complexity by ignoring the color channels and allowing the edge detection algorithm to focues solely on changes in blightness to detect lines. |
| ![solidWhiteCurve_Step02_blur] | The next step is to soften the image with a Gaussian blur. In this pipeline, the kernel was chosen to be 7. This is essentially a low-pass filter for the image that decreases the number of elements that get detected as an edge by the canny algorithm.|
|![solidWhiteCurve_Step03_canny]| The Canny algorithm is then applied to the softened image. The parameters used were an upper threshold of 120 and a lower threshold of 50. These values were chosen in a parameter search that sought to minimize edges detected over the whole image, while robustly detecting the lines of the lane markers over all of the given test material.|
|![solidWhiteCurve_Step04_mask]| The next step was to restrict the region of interest to a trapezoid covering the roadway directly in front of the vehicle. This shape was chosen to maximize the impact of the lane on the hough transform step, while minimizing the infulence of other edges created by vehicles in front or objects to the sides. The region was also constructed in terms of the image size so that the ROI would be robust against changes in image resolution.
|![solidWhiteCurve_Step05_masked]||
|![solidWhiteCurve_Step06_hough]| The Hough transform was then applied to the masked edges. This returns the line segments identified in the input image by looking at intersections in Hough space. Each pixel in the input image is a sine wave in Hough space and the location of an intersection of the sine waves of two pixels in Hough space corresponds to the line that passes through those two points in image space. When looking at all of the sine waves correspeonding to all the pixels in the masked edges image, regions of Hough space that have more intersections than a given threshold indicate the presence of a line. In this pipeline, Hough space was paritioned into bins with dimension rho=2 and theta=2 degrees and the threshold was 20 intersections. The pixels in that line are then grouped into line segments based on their contiguous length and gap parameters. In this pipeline, the minimum line length was 100 pixels and the maximum gap was 80 pixels.|
|![solidWhiteCurve_Step07_masked_hough]|The next step was to take the detected lines and determine the two lanes present in the image. This was done by first sorting the lines by their slope. Then spliting the list of lines wherever there was a gap between adjasent values of slope greater than a given threshold. This came from the observation that the lanes tended to generate a number of short line segments along their length  with slopes very close to the same value and that spurious lines due to noise tended to have significantly different slopes. The groups were then sorted according to the total length of line segments they contained and the top two were taken. In practice this did a very good job of picking the lane lines since they tended to generate many lines of moderate length comapred to the short lines generated by noise. The start points and end points of the line segments were avereaged respectively to generate a mean line segment for the lane which was then extended to the full length of the image. The original ROI mask is then applied.|
|![solidWhiteCurve_Step08_final]|The final step is to overlay the detected lines image onto the original image.|

### 2. Identify potential shortcomings with your current pipeline


While this pipeline performs well on the test images and video given, it falls apart quite badly on the challenge video. The biggest reason for this is due to the shadows that appear on the left lane-line and in that region of the video. The boundries of the shadow on the pavement create dozens of erroneous edges that confound those that indicate the location of the lane.

The lane finding pipeline is a small component of a self driving car system and its output is used in algorithms that actuate the controls of the vehicle. The system would have clear specifications for the interfaces between these components and in this case, clearly describe the way that the lanes should be described for blocks downstream. In this project, the output is a raster image that would be of little use to firther processing algorithms and is a major shortcoming of the pipeline as implemented in this project.

A trivial example of an output specification that would make sense for this pipeline is a two slope-intercept pairs that describe the two lines. But this is still a limitation because it conveys no information about the curvature of the lane ahead. Having lane curvature available would allow a control system to predect steering inputs necessary in the future and better manage the momentum of the vehicle.

In this pipeline, the two lanes are separated from other lines in the region of interest based on the assumption that they are the two longest, most well represented groups of lines detected by the canny edge detector. If there was anything infront of the ego vehicle, another car or truck, marking on the lane itself, or debris, it is likeley that the edges detected on that object would overwhelm the lines detected on the lane boundy and one of the two lane outputs would snap to the erroneous boject.


### 3. Suggest possible improvements to your pipeline

A big improvement could be acheived by utilizing the HSV color space and selecting the lanes based on hue and saturation while ignoring value. This would enable masking of large parts of the image that were not the yellow or white lane lines regardless of changing brightness or shadows. This change could lead to big improvements in performance on the challenge video.

An assumption that the positions of the lines in the video over time do not change very much from frame to frame. A stability improvement could be realized by incorporating a moving average that calculates the current lane line position using the current frame as well as the location of the lines in recent frames. This would prevent large jumps of the lane lines when noise defeats the algorithm and an incorrect result is given for a single frame. This failure mode is dominant in the current implementation when subjected to the challenge video, and this improvement would address that directly.

### 4. Closing remarks

This was a satisfying challenge and it forced me to focus on improving my tools and my process for iterative developemnt. I was most productive when I was able to code, test, and change quickly. This proved most valueable when tuning parameters for given functions such as Canny edge detection and the Hough transform line finder. I wrote many test jigs to explore the parameter space quickly and evaluate the results efficiently. A vestage of this is the DebugPipeline class seen in the project notebook. This made it very easy to dump test images to disk while the pipeline was executing and gave me visibility while I was developing it. That said, I found the "huristic" approach perscribed by this project tedious and fundamentally fragile. The techniques used are just that, techniques, and the resulting pipeline only as general as the sample set of images used during its development.
 