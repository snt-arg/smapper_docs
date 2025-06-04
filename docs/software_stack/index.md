# 🧠 Software Stack Overview {align="center"}

<p align="center">The brains of SMapper</p>

---

SMapper runs a robust and modular software stack designed to handle the full pipeline of **data acquisition**, **real-time processing**, **visualization**, and **storage**—all on-device, thanks to the onboard **Jetson AGX Orin Developer Kit (64GB)**.

The stack brings together **ROS 2 nodes**, **custom software packages**, and **web-based interfaces** to provide a streamlined experience for both field operations and remote monitoring.

---

## ⚙️ System Runtime

- **Base OS & Platform**  
  The system runs **JetPack 6.0**, chosen to maintain compatibility with the **e-Con Systems Argus cameras**, which require a **custom kernel** only supported by this version.

- **ROS 2 Distribution**  
  All robot-centric components operate on **ROS 2 Humble**, ensuring long-term support and compatibility across the ecosystem.

- **Sensor Data Flow**
    - Sensor drivers (LiDAR, cameras, IMUs) publish to ROS topics.
    - A **LiDAR odometry** node estimates pose in real-time.
    - Data is recorded into **ROS 2 bag files** for offline processing.

---

## 🌐 API & Web Interface

Alongside the ROS stack, SMapper hosts a **local hotspot** that runs:

- A **REST API** to control data capture, monitor system status, and trigger recordings.
- A **web application** for real-time visualization, interaction, and metadata management.
- This very **documentation site**, served locally for offline access in the field.

These services communicate with ROS through a **ROS bridge**, allowing real-time updates (e.g., live point clouds or pose trajectories) to appear directly in the browser.

---

## 🧩 Key Components

| Component             | Description                                                                 |
| --------------------- | --------------------------------------------------------------------------- |
| `ouster_ros`          | Official Ouster ROS 2 driver used to interface with the OS0-64 LiDAR.       |
| `argus_ros` (custom)  | Custom-built ROS 2 package for interfacing with 4x Argus 20MP cameras.      |
| `mola_lidar_odometry` | LiDAR odometry package used for pose estimation.                            |
| `rosbridge_server`    | WebSocket and JSON-based bridge for connecting the web app to ROS.          |
| `smapper_api`         | RESTful API that exposes control and status endpoints.                      |
| `smapper_app`         | Web dashboard and visualization layer built for touch-friendly interaction. |

---

## 🔍 Learn More

You can explore each software module in more detail:

- [SMapper API](./smapper_api.md)
- [SMapper Web App](./smapper_app.md)
- [Argus Cameras ROS Package](./argus_cameras_ros.md)
