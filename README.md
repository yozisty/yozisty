import cv2
import numpy as np
import pyautogui
import time

def left_click_when_color_detected():
    # Open the camera
    cap = cv2.VideoCapture(0)

    while True:
        # Read the frame from the camera
        ret, frame = cap.read()

        # Get the dimensions of the frame
        height, width, _ = frame.shape

        # Define the region of interest (ROI) as the center portion of the frame
        roi_x = int(width / 4)
        roi_y = int(height / 4)
        roi_width = int(width / 2)
        roi_height = int(height / 2)
        roi = frame[roi_y:roi_y+roi_height, roi_x:roi_x+roi_width]

        # Convert the ROI from BGR to HSV color space
        hsv_roi = cv2.cvtColor(roi, cv2.COLOR_BGR2HSV)

        # Define the lower and upper bounds for the color purple
        lower_purple = np.array([125, 50, 50])
        upper_purple = np.array([150, 255, 255])

        # Create a mask for the purple color range
        mask = cv2.inRange(hsv_roi, lower_purple, upper_purple)

        # Check if the mask contains any purple pixels
        if cv2.countNonZero(mask) > 0:
            # Perform a left click
            pyautogui.click()

        # Display the frame
        cv2.imshow('Color Detection', frame)

        # Exit the loop if 'q' is pressed
        if cv2.waitKey(1) & 0xFF == ord('q'):
            break

    # Release the video capture and close the windows
    cap.release()
    cv2.destroyAllWindows()

# Delay the execution to give time for the user to position the desired color in the middle of the screen
time.sleep(0.1)

# Run the left click program
left_click_when_color_detected()
