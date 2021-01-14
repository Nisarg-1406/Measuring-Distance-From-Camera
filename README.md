# Measuring-Distance-From-Camera
To measure the distance from the Camera

## Table of Contents - 
* [About Project](#about-project)
* [Detailed Explanation about Project](#detailed-explanation-about-project)
* [About Me](#about-me)

## About Project
This Project aims for the measuring the distance from the point to the camera and giving the distance as the output. The code is executed in python language in Jupyter Notebook. I am providing the detailed explanation of each and every line of the code. 

## Detailed Explanation about Project
* First install necessary libraries. 
  1) `from imutils import paths` - paths is used to load the available images in a directory. 
  2) `import numpy as np` - For numerical processing. 
  3) `import imutils` - To make basic image processing functions such as skeletonization, displaying Matplotlib images, sorting contours, translation, rotation, resizing with OpenCV
  4) `import cv2` - OpenCV.
  5) `import matplotlib.pyplot as plt` - To generate high quality line plots, scatter plots, histograms, bar charts. 
  
* Next is we define the `find_marker` function and it take one parameter - image. Taking example from **pyimagesearch.com** we are taking `8.5 x 11` inch piece of paper as our object and as we need to find the object, hence we would first convert this image into a grayscale image using `cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)` and then we would blur it using Gaussian Blur as `cv2.GaussianBlur(gray, (5, 5), 0)` so `cv2.gaussianblur()` function to apply Gaussian Smoothing on the input source image i.e `gray` in this case as input src image, then passing kernel size in the form of [height width] -> height and width should be odd and can have different values and finally we can detect the edges using canny edge detection. It is used for noise reduction. First argument is our input image. Second and third arguments are our minVal and maxVal respectively. After this edge of the paper i.e object is being clearly reveled.
    ```
    gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)
	  gray = cv2.GaussianBlur(gray, (5, 5), 0)
	  edged = cv2.Canny(gray, 35, 125)
    ```
* Next is to find the contours in the edged image and keep the largest one. So for this we use - `cv2.findContours(edged.copy(), cv2.RETR_LIST, cv2.CHAIN_APPROX_SIMPLE)`, so Contours can be explained simply as a curve joining all the continuous points (along the boundary), having same color or intensity. The contours are a useful tool for shape analysis and object detection and recognition. We use **RETR_EXTERNAL** (Contour Retrieval Mode) because it returns only the extreme outer flags i.e only the outer boundary. Now coming to `cv2.CHAIN_APPROX_SIMPLE`, If we pass `cv2.CHAIN_APPROX_NONE` - all the boundary points are stored, but we only need the endpoints, so it removes all redundant points and compresses the contour, thereby saving memory. Then we use that `imutils.grab_contours(cnts)` that grabs the appropriate tuple value based on whether we are using OpenCV 2.4, 3, or 4. Finally we would be computing the bounding box of the of the paper region and return it
    ```
    cnts = cv2.findContours(edged.copy(), cv2.RETR_LIST, cv2.CHAIN_APPROX_SIMPLE)
    cnts = imutils.grab_contours(cnts)
    c = max(cnts, key = cv2.contourArea)
    return cv2.minAreaRect(c)
    ```
