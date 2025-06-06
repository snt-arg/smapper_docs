# 🗃️ Datasets {align=center}

<p align="center">A collection of datasets recorded with SMapper</p>

---

## About a Dataset

The recorded datasets are saved as ROS 2 bag files[^1], a standard format for logging and replaying data within the ROS 2 ecosystem.
Each dataset contains the following published topics:

| Topic Name                      | Message Type              | Description                                                                 | Frequency ^\[Hz\]^ |
| :------------------------------ | :------------------------ | :-------------------------------------------------------------------------- | :----------------: |
| `/ouster/points`                | `sensor_msgs/PointCloud2` | 3D point cloud data captured by the Ouster OS0-64 LiDAR sensor.             |         10         |
| `/ouster/imu`                   | `sensor_msgs/Imu`         | Inertial measurements (accelerometer and gyroscope) from the Ouster IMU.    |        100         |
| `/camera/front_right/image_raw` | `sensor_msgs/Image`       | Raw image stream from the front-right high-resolution Argus camera.         |         30         |
| `/camera/front_left/image_raw`  | `sensor_msgs/Image`       | Raw image stream from the front-right high-resolution Argus camera.         |         30         |
| `/camera/side_right/image_raw`  | `sensor_msgs/Image`       | Raw image stream from the front-right high-resolution Argus camera.         |         30         |
| `/camera/side_left/image_raw`   | `sensor_msgs/Image`       | Raw image stream from the front-right high-resolution Argus camera.         |         30         |
| `/pose`                         | `geometry_msgs/Pose`      | Estimated 6-DoF pose generated by FastLIO, a LiDAR-based odometry solution. |         10         |
| `/tf`                           | `tf2_msgs/tfMessage`      | Transformation messages                                                     |         x          |

[^1]: Robot Operating System
