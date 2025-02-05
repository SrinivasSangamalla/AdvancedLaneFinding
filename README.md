## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

---

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



## [Rubric](https://review.udacity.com/#!/rubrics/1966/view) Points

### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---

### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Advanced-Lane-Lines/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!

### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

The code for this step is contained in the first code cell of the IPython notebook located in "https://github.com/SrinivasSangamalla/AdvancedLaneFinding/blob/main/Advanced%20Lane%20Finding.ipynb".

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 


### Pipeline Images

#### 1. Provide an example of a distortion-corrected image.

To demonstrate this step, I will describe how I apply the distortion correction to one of the test images like this one:
![alt text][https://github.com/SrinivasSangamalla/AdvancedLaneFinding/blob/main/Output_Images/download%20(1).png]

#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

I used a combination of gradient thresholds to generate a binary image (thresholding steps can be seen in the jupyter notebook).  Here's an example of my output for this step.  
I first used the Abstract Sobel Operator Threshold, then Magnitude Threshold then Direction Threshold.
Finally, I combined them all into a Combined Threshold Binary Image.

![alt text][https://github.com/SrinivasSangamalla/AdvancedLaneFinding/blob/main/Output_Images/download%20(6).png]

#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

The code for my perspective transform includes a function called `perspective_transform()`, which appears in cell 18 of the python jupyter notebook.  The `perspective_transform()` function takes as inputs an image (`img`), as well as source (`src`) and destination (`dst`) points.  I chose to hardcode the source and destination points in the following manner:

```python
src = np.float32([
        [258, 670], 
        [590, 450], 
        [690, 450], 
        [1041, 670]])
    
dst = np.float32([
        [258, 720], 
        [258, 0], 
        [1041, 0], 
        [1041, 720]])
```

This resulted in the following source and destination points:

| Source        | Destination   | 
|:-------------:|:-------------:| 
| 258, 670      | 258, 720      | 
| 590, 450      | 258, 0        |
| 690, 450      | 1041, 0       |
| 1041, 670     | 1041, 720     |

I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

![alt text][https://github.com/SrinivasSangamalla/AdvancedLaneFinding/blob/main/Output_Images/download%20(8).png]

I then verified the perspective transform on the binary image as well.

![alt text][https://github.com/SrinivasSangamalla/AdvancedLaneFinding/blob/main/Output_Images/download%20(12).png]

#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

The code for the 2nd order polynomial fitting is in cells 13-18 in the notebook. The results are as follows:

First, I took the histogram:

![alt text][https://github.com/SrinivasSangamalla/AdvancedLaneFinding/blob/main/Output_Images/download%20(9).png]

Here are the results:

![alt text][https://github.com/SrinivasSangamalla/AdvancedLaneFinding/blob/main/Output_Images/download%20(10).png]



#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

refer to cell 29 

#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

I implemented this in the last cell. The function is also used to create the video.
Here is an example of my result on a test image:

![alt text][https://github.com/SrinivasSangamalla/AdvancedLaneFinding/blob/main/Output_Images/download%20(11).png]

---

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](https://github.com/SrinivasSangamalla/AdvancedLaneFinding/blob/main/Test_Videos_Output/project_video_challenge1.mp4)

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

The approach I took is the following:
- Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
- Apply a distortion correction to raw images.
- Use color transforms, gradients, etc., to create a thresholded binary image.
- Apply a perspective transform to rectify binary image ("birds-eye view").
- Detect lane pixels and fit to find the lane boundary.
- Determine the curvature of the lane and vehicle position with respect to center.
- Warp the detected lane boundaries back onto the original image.
- Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

