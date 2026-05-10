# Aeropont

Solar Panel Inspection (Object Detection & Anomaly Detection)
The Solar Panel Inspection example demonstrates a two-stage AI pipeline. First, it identifies solar panels within an image using Object Detection, and then it analyzes those panels using Anomaly Detection to determine if they are clean or require maintenance (anomalous).
Description
This example showcases how to combine two specialized models for industrial inspection. The workflow involves uploading an image of a solar installation; the system first draws bounding boxes around all detected solar panels and then runs an anomaly detection model—trained on "clean" panel data—to flag any irregularities such as dirt, cracks, or shading.
The assets folder contains static images and a CSS style sheet for the web interface. In the python folder, we find the main script.
This example only uses the Arduino UNO Q CPU for running the application, as no C++ sketch is present in the example structure.
Bricks Used
The solar inspection example uses the following Bricks:
objectdetection: Brick to identify and locate solar panels within an image.
anomalydetection: Brick to classify whether the identified panel is "normal" (clean) or "anomalous".
web_ui: Brick to create a web interface.
Hardware and Software Requirements
Hardware
Arduino UNO Q (x1)
USB-C® to USB-A Cable (x1)
Personal computer with internet access
Software
Arduino App Lab
Note: You can also run this example using your Arduino UNO Q as a Single Board Computer (SBC) using a USB-C hub with a mouse, keyboard and monitor attached.
How to Use the Example
Run the app.
Open the app in your browser.
Upload an image of the solar panels you want to analyze.
Adjust the Confidence Threshold for the detection and the Anomaly Sensitivity if required.
Click the Run Inspection button.
View the results: solar panels will be highlighted with boxes, and an "Anomaly Detected" or "Clean" status will be displayed for the overall analysis.
How it Works
Once the application is running, you can access it from your web browser by navigating to <UNO-Q-IP-ADDRESS>:7000. At that point, the device begins performing the following:
Initial Setup:
Loads the object_detection, anomaly_detection, and web_ui Bricks.
Applies custom Arduino-themed CSS for styling to the web UI.
User Interface:
Split into two columns:
Left: Image upload area and the visual result showing bounding boxes and status overlays.
Right: Controls for detection confidence and action buttons:
Run Inspection
Run again
Change image
Detection & Analysis Execution:
Triggered when the user clicks Run Inspection.
Stage 1 (Detection): The model identifies the coordinates of solar panels.
Stage 2 (Anomaly): The identified regions are analyzed against the "Clean Panel" training data.
Results (Clean vs. Anomaly) are stored in session state and displayed on the page.
Understanding the Code
Here is a brief explanation of the application script (main.py):
Python
from arduino.app_utils import *
from arduino.app_bricks.web_ui import WebUI
from arduino.app_bricks.objectdetection import ObjectDetection
from arduino.app_bricks.anomalydetection import AnomalyDetection

# Initialize both AI models
detector = ObjectDetection()
anomaly_monitor = AnomalyDetection()
The function on_run_inspection performs the following:
Read inputs from the browser.
Run object detection to find the panels.
For the detected areas, run the anomaly detection model.
Determine the final state (Normal/Anomalous) based on the training threshold.
Send the annotated image and status back to the browser.
The App initialize the web interface, set up the endpoint and starts the runtime:
Python
ui = WebUI()
ui.on_message('run_inspection', on_run_inspection)
App.run()
Frontend Logic:
The (app.js) manages the browser-side logic of the App by doing the following:
Initializes page elements (upload area, preview, status indicators).
Handles image selection and converts it for the backend.
Connects via Socket.IO and sends the run_inspection request.
Receives inspection_result; if an anomaly is found, it changes the UI status color to alert the user.
Supports downloading the final report/image and resetting the view.
