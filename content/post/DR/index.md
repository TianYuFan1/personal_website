+++
author = "Tian Yu Fan"
title = "It Ends with Us"
date = "2022-10-01"
description = "Forward & inverse kinematics, velocity & torque control, trajectory planning, and computer vision"
tags = []
categories = []
series = ["Unified Robotics"]
aliases = []
image = "https://emanual.robotis.com/assets/images/platform/openmanipulator_x/OpenManipulator_Introduction.jpg"
+++

The main objective of this project was to redesign the WPI RBE3001 curriculum with the OpenManipulatorX. The WPI Robotics Engineering department has been continuously revising their RBE3001 hardware. The endless efforts ends with us.

# Tools
Software | Description
--------|------
Ubuntu 20.04 | Operating System
Robotic Operating System (ROS) | Software Framework
C++ | Programming Language
MATLAB | Software Framework
OpenCV | Computer Vision Toolbox

# Hardware
We are using the OpenManipulatorX arm, a 4-DOF robot arm with an U2D2 control. Dynamixels actuator the robot with integrated encoders for positional, velocity, and torque control.

![OpenManipulatorX](https://emanual.robotis.com/assets/images/platform/openmanipulator_x/OpenManipulator_Introduction.jpg)

# Software Design
## ROS/C++ Controller Interface
I designed the hardware interface in C++ based on the quality of the Dynamixel support library. The C++ controller writes joint angles to the robot and the gripper position. Additionally, it can read positional, velocity, and effort data from each joint of the robot. It also sets the velocity profile of the trajectory during movement.

This interface is a ROS node to support communication with multiple languages. This node allows users to specify the robot's desired state and read the robot's current state through ROS topics. 

## MATLAB Control Interface
MATLAB is the primary software for the control of autonomous behavior. These scripts process the forward & inverse kinematics, generate trajectories, capture joint data, visualize the data stream, and much more.

# Curriculum
The curriculum that we design will integrate the following concepts:
- Forward Kinematics using DH Tables
- Trajectory Generation 
- Inverse Kinematics
- Differential Kinematics (Velocity Kinematics)
- Computer Vision