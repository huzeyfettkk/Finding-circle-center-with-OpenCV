# Finding-circle-center-with-OpenCV
In this project, you can detect the target circles and their centers.
I obtained the outline of this project from "https://medium.com/@isinsuarici/hough-circle-transform-in-opencv-d74bdf5161ed". 
The code trained using the OpenCV library on that site could only find circles and their centers in photos. 
Here, I made the necessary improvements to the code to enable it to perform detections live and actively on a camera, not just on photos.

# Installation
Make sure you have Python installed. Clone the repository and install the required libraries using pip:
```python
pip install opencv-python
pip install numpy
```
# Usage
Run the "circle_center_detection.py" file:
```python
python circle_center_detection.py

```
Press the 'q' key to exit the program.
# How it Works
This program uses OpenCV to capture frames from the webcam and then applies image processing techniques to detect circles using the Hough Circle Transform. 
Detected circles and their centers are then drawn on the live video feed.
# Acknowledgements
The outline of this project was obtained from this Medium article.

# Code
Just paste this into the ".py" extension file you opened and run it.
```python
# Imports the OpenCV library with an alias "cv"
import cv2 as cv
# Imports the NumPy library with an alias "np"
import numpy as np

# Initializes the webcam. The argument "0" selects the first webcam.
cap = cv.VideoCapture(0)

# Creates an infinite loop to process each frame.
while True:
    # Reads the next frame into the frame variable and saves whether the read operation was successful in the "ret" variable.
    ret, frame = cap.read()

    # If the frame could not be read successfully, exits the loop.
    if not ret:
        print("Can't receive frame (stream end?). Exiting ...")
        break

    # Applies Gaussian blur to the image to reduce noise.
    gray = cv.cvtColor(frame, cv.COLOR_BGR2GRAY)

    # Apply Gaussian Blur
    gray_blurred = cv.GaussianBlur(gray, (7, 7), 1.5)

    # Detects circles using the Hough Circle Transform. Parameters param1 and param2 adjust the detection sensitivity.
    circles = cv.HoughCircles(gray_blurred, cv.HOUGH_GRADIENT, 1.3, 30, param1=150, param2=70, minRadius=0, maxRadius=0)

    # Initialize cimg as a copy of the frame before drawing on it
    cimg = frame.copy()

    # Copies the original frame before drawing on it.
    if circles is not None:                           # If circles are detected
        circles = np.uint16(np.around(circles))       # Converts the circle coordinates to integers.

         # Iterates over each circle.
        for c in circles[0, :]:    # Iterates over each circle.
            # Draws the circle's circumference in green.
            cv.circle(cimg, (c[0], c[1]), c[2], (0, 255, 0), 3)

            # Draws a small circle (radius 1) at the center of the circle in red.
            cv.circle(cimg, (c[0], c[1]), 1, (0, 0, 255), 5)

    #  Displays the image with detected circles.
    cv.imshow('Webcam', cimg)

    # Breaks the loop and exits the program if the 'q' key is pressed.
    if cv.waitKey(1) & 0xFF == ord('q'):
        break

# Release the webcam and close the window
cap.release()
cv.destroyAllWindows()

```

