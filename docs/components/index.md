# 🧩 Components {align="center"}

<p align="center">🔎 What's Inside? 👀</p>

---

<figure markdown="1">
![Components Explosion giff](../assets/gifs/smapper_explosion_1080p.gif){width=80% align=center}
</figure>

To support its high-performance, real-time capabilities, SMapper is built with a carefully selected set of components, each playing a crucial role in data acquisition and processing. The exploded view below offers a peek inside the device:

![Components Explosion](../assets/figures/smapper_explosion_w_details.png){width=100% align=center}

## 🔧 Core Components:

-   **Jetson AGX Orin Developer Kit**[^1]

    The brain of the system, handling real-time sensor synchronization, data capture, and onboard processing.

-   **4x 20MP e-CAM200_CUOAGX by e-Con Systems**[^2]

    Featuring 118.81° FoV and rolling shutter sensors, these high-resolution cameras are synchronized and arranged for overlapping coverage.

-   **Ouster OS0-64 LiDAR**[^3]

    A compact yet powerful 3D LiDAR unit providing dense point clouds, ideal for close-range mapping and SLAM integration.

-   **Intel RealSense D435i**[^4]

    Adds RGB-D data and IMU readings, expanding the sensory depth for cases only relying on RGB-D datasets.

## 🔋 Power & Connectivity

-   **20V 6.0Ah Drill Battery**

    Easily swappable and rechargeable—ideal for long field sessions.

-   **Buck Converter**

    Steps down battery voltage to power the Jetson and peripherals efficiently.

-   **DC-Jack to USB-C Male Adapter**

    Delivers power to the Jetson using standard interfaces.

-   **USB 3.0 Right-Angle Adapter**

    For compact, secure, and clean cable management inside the enclosure.

## 🛠️ Design & Mounting System

SMapper is designed not only for performance, but for usability in the field. Its custom-built physical design brings together modularity, thermal efficiency, and versatility in deployment, whether handheld or robot-mounted.

### 3D-Printed Enclosure

The body of SMapper is split into three parts which are bolted together to form a robust enclosure:

-   Bottom shell – Houses the power module and the Jetson AGX Orin.

-   Middle shell – The main body of the enclosure, where the cameras, including the realsense are mounted to.

-   Top shell – Optimized for SLM metal 3D printing, this part helps dissipate heat from the LiDAR during continuous operation.

📷 (Insert close-up of the enclosure with shell labels here)

### Coupling & Attachment Mechanism

The base of the system features a custom sliding coupling that enables quick attachment/detachment to different platforms:

-   Handheld configuration with the ergonomic grip and battery mount.
-   Robot-mount configuration, adaptable to a variety of mobile bases, including legged robots.

📷 (Insert photos of the coupling system sliding into both grip and robot mount here)

### Ergonomic Handle

Designed with field use in mind, the handle balances the weight of the ~2kg device evenly thanks to its integration with a standard 20V drill battery, keeping the center of mass low and making the device easy to carry and operate during long sessions.

📷 (Insert photo of the handheld setup in action here)

[^1]: https://developer.nvidia.com/embedded/learn/jetson-agx-orin-devkit-user-guide/index.html
[^2]: https://www.e-consystems.com/nvidia-cameras/jetson-agx-orin-cameras/20mp-ar2020-high-resolution-mipi-camera.asp
[^3]: https://ouster.com/products/hardware/os0-lidar-sensor
[^4]: https://www.intelrealsense.com/depth-camera-d435i/
