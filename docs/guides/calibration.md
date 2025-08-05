# 📐 Device Calibration

Device calibration is a critical step for accurate sensor fusion and robust performance.
This project uses the **Kalibr toolbox** to calibrate the intrinsic and extrinsic parameters
of each camera. We also estimate the IMU noise model for the Ouster OS0 using
Allan variance analysis and compute the spatial transformation between each camera and the OS0's IMU frame.

To streamline this process, we have developed the [smapper_toolbox](https://github.com/snt-arg/smapper_toolbox/tree/main), an automated calibration toolbox that significantly reduces setup time and ensures reproducibility.

## Pre-requisites

A calibration target is required to perform the calibration. The recommended target
is the `Aprilgrid 6x6 0.8x0.8 m (A0 page)`, which can be downloaded [here](https://github.com/ethz-asl/kalibr/wiki/downloads#calibration-targets).
For best results, mount the target on a rigid panel with a matte finish to prevent reflections.

## Calibration Process

### Data Collection

1.  **Position the calibration target.** Place it vertically, for instance, against a wall, at a height that is comfortable for data collection.
2.  **Collect IMU data.** Record a rosbag containing **only IMU data** from both the Ouster and Realsense IMUs. It is recommended to collect at least **3 hours** of data, but more is better (e.g., 20 hours is more than sufficient). Place the device in a static position and record the IMU data for each sensor independently.
3.  **Collect dynamic data for each camera.** For each camera, including the Realsense RGB sensor, record a dynamic rosbag. Each rosbag should contain one camera stream and the Ouster and/or Realsense IMU data.
    - **Procedure:** Start recording with the camera facing the target. Exert all axes of the IMU by rotating the device on its pitch, roll, and yaw axes. Then, perform translational movements: up and down, left to right, and forward and backward. Try to keep the target in view at all times. This [video tutorial](https://www.youtube.com/watch?v=puNXsnr2_h4) provides an excellent demonstration of the process.
4.  **(Optional) Collect stereo camera data.** You can collect another rosbag with the two front cameras and one or more IMU sources. This is useful for stereo vision applications. Repeat the procedure from the previous step, ensuring that both cameras capture some of the same features.

### Folder Structure

After collecting all the bags, organize them into the following folder structure:

```
.
└── ros2
    ├── calib02_front_left
    ├── calib02_front_right
    ├── calib02_side_left
    ├── calib02_side_right
    └── imu16h_imu
```

!!! warning

    This folder structure is crucial for the automation toolbox to function correctly.
    You must have a folder named `ros2` containing the ros2 bags. The toolbox will then create a `ros1` folder with the converted bags for Kalibr.

### Using the `smapper_toolbox`

!!! danger "Important"

    You must have the `smapper` repository on your system, as it contains required files.
    Cloning it also clones the toolbox. Additionally, `smapper_toolbox` uses the **UV Python package manager**.
    Please refer to the [smapper_toolbox documentation](../software_stack/smapper_toolbox.md) for more information.

    **⚠️ These following steps must be performed on an X86 CPU due to docker image constraints!**

1.  **Navigate to the `smapper_toolbox` directory**, which should be located at `smapper/software/smapper_toolbox`.
1.  **View the help menu** by running `uv run ./toolbox --help`.
1.  **Modify configuration** accordingly to your needs. Configuration lives in `smapper_toolbox/config/config.yaml`
1.  **Run the calibration pipeline** with the following command:
    ```bash
    # The path to the rosbags is the root directory where the ros2 folder is located.
    # You can also run --help to see all available options.
    uv run ./toolbox kalibr all --rosbags-dir path/to/where/rosbags/are/
    ```
    This command executes a two-stage process. First, it converts all ROS2 bags to ROS1 bags, which can take some time depending on their size. This creates a `ros1` folder next to the `ros2` folder. Then, it proceeds to the Kalibr stage, where it extracts basic metadata from the available bags, runs the intrinsics calibration for each bag containing one or more image topics, computes the noise model of the IMUs (if there are bags with sufficient IMU data), and finally performs the camera-IMU calibration. All results are saved in the calibration directory specified in the configuration file.
1.  **Generate the transformation tree.** This step requires manual intervention. You will need to go through each camera's result from the camera-IMU step and fill in the following [file](https://github.com/snt-arg/smapper_toolbox/blob/main/config/transform_input_example.yaml).
1.  **Generate the transformations.** Once you have filled in the file, run the following command:
    ```bash
    uv run ./toolbox transforms generate --input the_file.yaml --output output_tfs.yaml
    ```
    This command reads the file and generates the transformations so that all sensors are referenced to a common `base_link`. It also generates a ROS launch file that can be used directly to publish all the transformations.
1.  **Final steps.** Copy the launch file to the actual device in `smapper/software/ros_ws/src/smapper_bringup/launch` and create or update the `camera_info` files located in `smapper/software/ros_ws/src/smapper_bringup/config/calib`.
