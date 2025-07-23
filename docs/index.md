# 🗺️ SMapper {align=center}

<p align="center"><i>A Portable Multimodal Sensing Platform Desiged for Robotics Research</i></p>

---

<figure markdown="1">
![showcase gif](./assets/gifs/smapper_showcase.gif){width=80% align=center}
</figure>

**SMapper** is a portable, multimodal sensor platform designed from the ground up
to accelerate robotics research, particularly for Simultaneous Localization and Mapping (SLAM) applications.
It was created to bridge the gap between expensive, inflexible commercial systems
and unreliable do-it-yourself solutions, offering a versatile, high-performance,
and user-friendly tool for collecting high-quality, time-synchronized sensor data.

This documentation serves as the central resource for understanding, operating, and developing with the SMapper platform.

## The Platform at a Glance

SMapper is an integrated system of carefully selected hardware and custom-built software
designed for reliability and ease of use in research environments.

- **Hardware**: The core of the device features a powerful NVIDIA Jetson Orin AGX processing unit.
  For sensing, it is equipped with a 3D LiDAR with a wide vertical field of view,
  a panoramic array of four high-resolution 20MP cameras, and an Intel RealSense depth/inertial camera.
  The entire system is housed in a robust, 3D-printed enclosure and powered by a field-swappable battery,
  enabling approximately one hour of continuous operation.

- **Software**: A complete, full-stack application running on ROS 2 Humble provides
  total control over the platform. This includes a custom, high-performance C++ camera driver (`argus_camera_ros`)
  for precise data acquisition, a backend RESTful API (`smapper_api`) for system control,
  and an intuitive React web interface (`smapper_app`) for monitoring, data management, and visualization.

- **Calibration**: A key feature of the project is a rigorous, automated calibration pipeline.
  Using the custom `smapper_toolbox`, the system can accurately determine the intrinsic
  and extrinsic parameters of all sensors, generating a ready-to-use configuration for high-precision data fusion.

## Purpose of This Documentation

This site is intended to provide all the necessary information for users and developers. Here you will find:

- **Getting Started Guides**: Step-by-step instructions for setting up, operating, and collecting data with SMapper.

- **System Architecture**: Detailed descriptions of the hardware components, mechanical design, and software stack.

- **Software Documentation**: In-depth information on the custom software components, including the API endpoints and driver configurations.

- **Calibration Pipeline**: A complete guide to using the automated `smapper_toolbox` to calibrate the device.

Whether you are a new user looking to collect your first dataset or a developer aiming to extend the platform's capabilities, this is the place to begin.
