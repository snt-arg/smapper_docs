# SMapper Light Dataset

SMapper light is a [publicly available](https://huggingface.co/datasets/snt-arg/smapper-light/tree/main) multimodal dataset collected using the **SMapper** platform,
an open-hardware, multi-sensor device designed for SLAM (Simultaneous Localization and Mapping) research.
The dataset provides 3D LiDAR, multi-camera, and IMU measurements, enabling benchmarking of visual, LiDAR, and visual–inertial SLAM methods.

## 🚀 Applications

SMapper light dataset can be used for:

- Benchmarking LiDAR SLAM frameworks (e.g., Fast-LIO, S-Graphs)
- Benchmarking Visual SLAM frameworks (e.g., ORB-SLAM3, vS-Graphs)
- Benchmarking Multimodal SLAM frameworks

## 📊 Dataset Overview

| Scenario    | Instance                                                                                             | Duration  | Size on Disk | Description                          |
| ----------- | ---------------------------------------------------------------------------------------------------- | --------- | ------------ | ------------------------------------ |
| **Indoor**  | [`IN_SMALL_01`](https://huggingface.co/datasets/snt-arg/smapper-light/blob/main/IN_SMALL_01.zip)     | 01 m 29 s | 5.7 GB       | Confined single-room environment     |
|             | [`IN_MULTI_01`](https://huggingface.co/datasets/snt-arg/smapper-light/blob/main/IN_MULTI_01.zip)     | 06 m 46 s | 13.5 GB      | Multi-room linear trajectory         |
|             | [`IN_MULTI_02`](https://huggingface.co/datasets/snt-arg/smapper-light/blob/main/IN_MULTI_02.zip)     | 07 m 07 s | 13.0 GB      | Multi-room with loop closure         |
|             | [`IN_LARGE_01`](https://huggingface.co/datasets/snt-arg/smapper-light/blob/main/IN_LARGE_01.zip)     | 09 m 30 s | 57.9 GB      | Large-scale indoor with loop closure |
| **Outdoor** | [`OUT_CAMPUS_01`](https://huggingface.co/datasets/snt-arg/smapper-light/blob/main/OUT_CAMPUS_01.zip) | 04 m 57 s | 36.4 GB      | Urban campus linear path             |
|             | [`OUT_CAMPUS_02`](https://huggingface.co/datasets/snt-arg/smapper-light/blob/main/OUT_CAMPUS_02.zip) | 05 m 15 s | 37.8 GB      | Urban campus circular path           |
| **Total**   |                                                                                                      | 35 m 04 s | 164.3 GB     |                                      |

!!! info

    The available topics can be found in [here](./index.md)
