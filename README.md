Advanced Lane Finding


In this project, your goal is to write a software pipeline to identify the lane boundaries in a video, but the main output or product we want you to create is a detailed writeup of the project.  Creating a great writeup:

The goals / steps of this project are the following:

* Compute the **camera calibration matrix and distortion coefficients** given a set of chessboard images.
* Apply a **distortion correction** to raw images.
* Use color transforms, gradients, etc., to create a thresholded **binary image**.
* Apply a **perspective transform** to rectify binary image ("birds-eye view").
* **Detect** lane pixels and **fit** to find the lane boundary.
* **Determine the curvature** of the lane and vehicle position with respect to center.
* **Warp back** the detected lane boundaries onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.



[image1]: ./output_images/undistort_output.png	"Undistorted"
[image2]: ./output_images/binary_combo.png	"Binary Example"
[image3]: ./output_images/test2.jpg	"original"
[image4]: ./output_images/perspective.png	"Perspective"
[image5]: ./output_images/color_fit_lines.png	"Fit Visual"
[image6]: ./output_images/example_output.png	"Output"
[video1]: ./project_video_output.mp4



### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 



Distorted Chessboard images and undistorted one through camera calibration

![undistored][image1] 



### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.

To demonstrate this step, I will describe how I apply the distortion correction to one of the test images like this one:![original][image3]



#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

I used a combination of color and gradient thresholds to generate a binary image. Here's an example of my output for this step. 

For images taken from recorded camera mounted on the car,  apply distortion correction and create thresholded binary images

![binary images][image2]

#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

The code for my perspective transform includes a function called `perspectiveview()`, which appears in  the 4th code cell of the IPython notebook `Advanced_Lane_Lines_demo.ipynb` . The `perspective()` function takes as inputs an image (`img`), as well as source (`src`) and destination (`dst`) points.  

I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

I masked the region of interest (the dashed polygon in the left figure) and performance perspective transform within the region (change from horizontal view to top view)

![perspective_transform][image4]

#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

Detecting the lane lines within the region of interest from the pespective image using **sliding window techniques** and then I did some other stuff and fit my lane lines with a 2nd order polynomial interpolation to fit the line functions.

![fit lane][image5]



#### 5. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

I implemented this step in  23th code cell of the IPython notebook `Advanced_Lane_Lines_demo.ipynb`  in the function `map_back()`. I wrap back the detected lane lines to the original images and display the detected lane region overlaid on the original images.

![output][image6]

------

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](./project_video_output.mp4)

------

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

For most of the video, the lane lines are detected correctly. However, sometimes for the adjacent frames in the video, there're some discontinue in the detected lane lines (not smoothly changing) due to the light shades, road conditions etc.  For further improvement, I'm going to track few adjacent frames and do some averaging/smoothing to remove such jumps.