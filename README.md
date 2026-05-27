# Autonomous Driving Pipeline in ROS — Segway Loomo

Open-source modular autonomy stack for mobile robots in ROS, developed at **[EPFL VITA Lab](https://www.epfl.ch/labs/vita/)** under Dr. Alexandre Alahi. Targets a **Segway Loomo** indoor mobile robot; framework generalizes to other platforms.

> **Status:** archived (last active 2023-06). Reference implementation — the modular structure (detection → tracking → keypoints → control) holds up; the underlying perception models (YoloV5, Stark, MMTracking) and ROS 1 / Catkin workspace are pinned to the 2023 stack. A modernized successor lives in [perception-pipeline](https://github.com/thrmnn/perception-pipeline).

<p float="left">
  <img src="./img/ADP.gif" width="400" />
  <img src="./src/control/Images/MR_Loomo_closed_loop.gif" width="400" />
</p>

## Description

We propose a modular pipeline for autonomous systems, which combines Computer Vision algorithms leveraging Deep Learning with classic robust methods for Path Planning, State Estimation and Control tasks. Our structure is designed to be general to adapt to different types of mobile robots and also flexible to easily change algorithms in its various independent modules. 

We use [Robot Operating System (ROS)](https://www.ros.org/) and network socket to establish wireless connection between the Server (Computer running the pipeline) and the Client (Mobile Robot).

For our implementation we use a [Loomo Segway Robot](https://store.segway.com/segway-loomo-mini-transporter-robot-sidekick).

Finally in our case we specifically adapt this general Autonomous Driving framework to fit the needs of neuroscience research area by precisely estimating the 3D Pose of the target, helping paraplegic patients to walk using our Loomo mobile robot as an assistant.

## User Guide

Before using the pipeline be sure to have completed the [🛠️ Install](./Install.md) process.


### Quick Start

In the root folder, ROS request that we type the following command everytime before launching any package.

```shell
source devel/setup.bash
```

If we need to change some parameters of the ROS structure, we can go to [Loomo.launch](/src/loomo) inside the launch folder.

The Autonomous System used in our work is a [Loomo](https://store.segway.com/segway-loomo-mini-transporter-robot-sidekick) Segway Robot. We need beforehand to install a specific Android app to enable the connection between the client (Loomo) and the server (ROS pipeline). The app source code and package can be found [here](https://github.com/theoh-io/Loomo_app_ADP).

Once everything is set up and adjusted to our specific context, we are ready to launch:

```shell
roslaunch loomo Loomo.launch
```


### Structure

We present all the different pillars of the pipeline and a brief explanation of each one.

```
Autonomous Pipeline
│
│─── Loomo ───> Package which contains the launch file with all configurable parameters. Also contains the tools (functions common to every modules)
│
│─── Message Types ───> Package which includes all types of messages for topics.
│
│─── Perception ───> Package and node to robustly detect and track from cameras and depth sensors. Perception package is modular enabling for change in used configurations.
|       |
|       |─── Detectors ───> Person Detection algorithms (Yolov5).
|       |
|       |─── Trackers ───> Single Object Tracker running on top of detection to robustly track the person of Interest (Stark).
|       |
|       |─── Keypoints ───> 2D/3D keypoints Estimation to get the position of the link of the target in real time.
|       |
|       |─── Perceptors ───> High Level Class where we combine all the perception module providing simple call to perception.
│
│─── Estimation ───> Package to estimate current state from sensors information.
│       │
│       │─── Robot State ───> Node to estimate the state from IMU and GPS.
│       │
│       └─── Map State ───> Node to generate a map and estimate the state from all sensors.
│
│─── Prediction ───> Package and node to generate trajectory prediction of the detected person.
│
│─── Path Planning ───> Package and node to calculate robot's desired path to avoid collision.
│
│─── Control ───> Package and node to compute control commands using Model Predictive Control and ensure that the robot is following the desired path.
│
└─── Visualization ───> Plotting tools for all nodes current data.
```

## Acknowledgements

This pipeline builds on the prior [Autonomous Driving Pipeline](https://github.com/cconejob/Autonomous_driving_pipeline) by C. Conejo Bernal. The companion Android bridge for the Loomo client lives at [thrmnn/Loomo_app_ADP](https://github.com/theoh-io/Loomo_app_ADP) (mirrored — original on theoh-io).

The full VITA Lab technical report is bundled as [paper.pdf](/paper.pdf) — design rationale, ablation results, and a deeper walkthrough of the application to paraplegic-patient walking assistance.

---

## License

MIT — see [LICENSE](./LICENSE).

If you build on this for academic work, a citation is appreciated:

```
Hermann, T. A. (2023). Modular Autonomous Driving Pipeline in ROS — applied to
mobile assistance robots. EPFL VITA Lab, under Dr. Alexandre Alahi.
```




