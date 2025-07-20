# 📐 Device Calibration

Device calibration is a critical step to ensure accurate sensor fusion and robust performance. For this project, we use the Kalibr toolbox to calibrate the intrinsic and extrinsic parameters of each camera.

We also estimate the IMU noise model for the Ouster OS0 using Allan variance analysis and compute the spatial transformation between each camera and the OS0's IMU frame.

To streamline the process, we have developed an automated calibration script that runs inside a Docker container. This automation greatly reduces setup time and ensures reproducibility.

🗃️ The rosbags used for calibration are available on the Datasets page.
