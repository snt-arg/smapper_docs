# About {align=center}

<p align="center">A collection of datasets recorded with SMapper</p>

---

Datasets recorded with **SMapper** are stored as ROS 2 bags. This means they
follow a standard format, are easy to play back, and can be used with many
existing tools that already support rosbags or can convert them into other formats.

All datasets here are stored using the **MCAP** format. This is now the default in
ROS 2 Jazzy and brings some advantages compared to the traditional SQLite storage backend.

If you’re on an older ROS 2 distribution, you might need to install MCAP support manually.
You can install the package like this:

```bash
sudo apt install ros-$ROS_DISTRO-rosbag2-storage-mcap
```

## Topics in Each Dataset

Every dataset includes the following topics. These cover 3D point clouds, IMU data from two different sources, multiple camera streams, and the required transforms.

| Topic Name                                           | Message Type              | Description                                                      | Frequency [Hz] |
| :--------------------------------------------------- | :------------------------ | :--------------------------------------------------------------- | :------------: |
| `/ouster/points`                                     | `sensor_msgs/PointCloud2` | 3D point cloud data captured by the Ouster OS0-64 LiDAR.         |       10       |
| `/ouster/imu`                                        | `sensor_msgs/Imu`         | Accelerometer and gyroscope data from the Ouster IMU.            |      100       |
| `/camera/realsense/imu`                              | `sensor_msgs/Imu`         | Accelerometer and gyroscope data from the Realsense D435i IMU.   |      400       |
| `/camera/realsense/depth/image_raw`                  | `sensor_msgs/Image`       | Raw depth images from the Realsense D435i.                       |       30       |
| `/camera/realsense/depth/camera_info`                | `sensor_msgs/CameraInfo`  | Camera info for the depth stream of the Realsense D435i.         |       30       |
| `/camera/realsense/color/image_raw`                  | `sensor_msgs/Image`       | Raw color images from the Realsense D435i.                       |       30       |
| `/camera/realsense/color/camera_info`                | `sensor_msgs/CameraInfo`  | Camera info for the color stream of the Realsense D435i.         |       30       |
| `/camera/realsense/aligned_depth_to_color/image_raw` | `sensor_msgs/Image`       | Depth images aligned to the color stream of the Realsense D435i. |       30       |
| `/camera/front_right/image_raw`                      | `sensor_msgs/Image`       | Raw images from the front-right high-resolution Argus camera.    |       30       |
| `/camera/front_left/image_raw`                       | `sensor_msgs/Image`       | Raw images from the front-left high-resolution Argus camera.     |       30       |
| `/camera/side_right/image_raw`                       | `sensor_msgs/Image`       | Raw images from the side-right high-resolution Argus camera.     |       30       |
| `/camera/side_left/image_raw`                        | `sensor_msgs/Image`       | Raw images from the side-left high-resolution Argus camera.      |       30       |
| `/camera/front_right/camera_info`                    | `sensor_msgs/CameraInfo`  | Camera info for the front-right high-resolution Argus camera.    |       30       |
| `/camera/front_left/camera_info`                     | `sensor_msgs/CameraInfo`  | Camera info for the front-left high-resolution Argus camera.     |       30       |
| `/camera/side_right/camera_info`                     | `sensor_msgs/CameraInfo`  | Camera info for the side-right high-resolution Argus camera.     |       30       |
| `/camera/side_left/camera_info`                      | `sensor_msgs/CameraInfo`  | Camera info for the side-left high-resolution Argus camera.      |       30       |
| `/tf`                                                | `tf2_msgs/tfMessage`      | Transformation frames (dynamic).                                 |       –        |
| `/tf_static`                                         | `tf2_msgs/tfMessage`      | Transformation frames (static).                                  |       –        |
