# **Finding Lane Lines on the Road** 

### Reflection

### Pipeline description
My pipeline has the following 7 steps (see pictures above presented for the test images):
* Selecting a color range for distinguishing yellow and white shades on the road and mask all other colors
    * We change the color space of the image to HSV
    * Apply OpenCV's inRange function for color selection
    * We have used the color range [0, 0, 220] to [255, 255, 255]
	[image1]: ./output_images/solidWhiteCurve_color_selected.jpg "Grayscale"
* Converting the color-selected image into grayscale for easier edge detection
	[image2]: ./output_images/solidWhiteCurve_gray.jpg "Grayscale"
* Applying Gaussian blur on the grayscale image to normalize edges and sharpness
	[image3]: ./output_images/solidWhiteCurve_blur.jpg "Grayscale"
* Detecting canny edges on the normalized color-selected grayscale image
	[image4]: ./output_images/solidWhiteCurve_canny.jpg "Grayscale"
* Applying region of interest mask to select polgonal area containing the road only
	[image5]: ./output_images/solidWhiteCurve_masked.jpg "Grayscale"
* Applying Hough transform to find the line segments in the image/frame and draw lines in-place
    * The image_draw_lines does the following steps
        * separates the line segments based on the slope to be on the left and right lanes
        * drops line segments that are out-of specified range for the angle of slope
        * Fits the points to a straight-line (degre-1 polynomial) for both left and right lanes
        * Computes the point of intersection between both the straight lines
        * Extrapolates the lines from the bottom of the image to the intersecting point to cover the visible area between the lanes
	[image6]: ./output_images/solidWhiteCurve_hough.jpg "Grayscale"
* The blank image with lane lines highlighted in red is then superimposed on the original image
	[image7]: ./output_images/solidWhiteCurve_merged.jpg "Grayscale"

Note:
* It took quite some time to prepare the pipeline for the challenge
* The color mask on HSV did the trick
* We use a triangular bounded region where the lanes meet at the horizon
* The pipeline works satisfactorily on all example images and videos

### Shortcomings in the current pipeline
The current pipeline has the following shortcomings:
* It does not detect the curvature of the lanes
* Consecutively missing lane lines may open up unsafe regions for the car
* Safety corner cases are are not extensively considered

### Possible improvements
Here are a few possible ways to improve the pipeline:
* We may use a quadratic polynomial to fit the to detect the curvature and directions of the lanes
* We may use regression to extrapolate missing lines
