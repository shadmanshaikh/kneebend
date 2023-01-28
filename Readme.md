# Knee Bend Exercise Counter


## NOTE: All the [fluctuation information](https://github.com/shadmanshaikh/kneebend/blob/master/all_the_fluctuating_frame/KneeBend-Scenes.csv) , [clips](https://github.com/shadmanshaikh/kneebend/tree/master/fluctuation_detected_vid) and [fluctuating frames](https://github.com/shadmanshaikh/kneebend/tree/master/all_the_fluctuating_frame) are present in the repo for viewing

## STEPS:
* Remove duplicate frames 
* Configuring the Media Pipe
* Estimating poses
* Extracting joint coordinates
* Calculating angles between joints
* Inserting timer, stage (bent or relaxed), rep counter, and feedback

## 0. Remove duplicate frames [method 1]:
* Extract the individual frames of the video using a video processing library like OpenCV.
* Once the frames are extracted, Python's built-in **hashlib** module is to compute a hash value for each frame, and then compare the hash values to identify duplicate frames.


## 1. Detecting fluctuations in the BendVideo.mp4 [method 2]:

**Note : Install [pySceneDetect](https://scenedetect.com/en/latest/) to overcome the fluctuations, this basically helps in detecting the cut scenes in the video** 
* To use pySceneDetect [ for opencv ] :
`` 
pip install --upgrade scenedetect[opencv]
``
* To use pySceneDetect [ for opencv headless ] :
``
pip install --upgrade scenedetect[opencv-headless]
``

* Use the following command to check the cut scenes in the video :
`` scenedetect --input KneeBendVideo.mp4 detect-adaptive list-scenes save-images ``

* Also Install ffmpeg for rendering the different frame :
`` 
pip install ffmpeg-python
``
or install [ffmpeg-git-full.7z](https://www.gyan.dev/ffmpeg/builds/)




## 2. Configuring the Media Pipe:
* Install and import [Mediapipe](https://google.github.io/mediapipe/solutions/pose), it is a cross-platform library developed by Google that provides amazing, ready-to-use ML solutions for computer vision tasks.
* Along with Mediapipe, install and import some other dependencies such as **OpenCV** and **NumPy**.


## 3. Estimating poses:
* In this step, we will be estimating all the different joints and parts within our body.
* Capture the video feed from the [video file](https://github.com/Jeevesh28/Knee-Bend-Counter-Mediapipe/blob/main/KneeBendVideo.mp4) provided.
* Recolor our image because when we pass the image to mediapipe it should be in RGB format, which is the default BGR when we read it.
* Use the Pose estimation model to detect the pose.
* Recolor the image back to the default BGR format.
* Perform detections, i.e., draw **landmarks** from the video feed (e.g., nose, eyes, ears, shoulders, elbows, wrists).

## 4. Extraction of Joint Coordinates:
* Use the pose estimation model to extract landmarks using detected pose estimation, as we did in the previous step.
* Extract the landmarks for the main 3 joints that we need to calculate the rep count for knee-bending exercises which are **hip**, **knee**, and **ankle**.

## 5. Calculating angles between joints:
* Calculate the **angle** between the hip, knee, and ankle to identify whether the leg is straight or bent.
* For calculate the angle, we are going to calculate the radians with the help of three parameters passed to the function **calculate_angle()**, which are hip, knee, and ankle, by using a trigonometric function and then converting radian to angle.

## 6. Inserting timer, stage (bent or relaxed), rep counter, and feedback:
* If the calculated angle is less than or equal to **140&deg;**, then the stage of the leg will be **bent**, and else if the calculated angle is greater than 140&deg; and the stage is bent, then the stage will be **relaxed**.
* A timeholder is used to measure the start time in starting when the stage changes from relaxed to bent stage, and the end time is measured when the stages changes from bent to relaxed stage, duration is calculated by subtracting the end time from the start time. 
* If a user can hold the leg in the bent stage for the duration of **8** or more than 8 seconds, then the rep count is incremented. 
* Otherwise if the user is unable to stay in the bent stage for more than 8 seconds, the feedback will print **keep your knee bent**.
