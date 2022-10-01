+++
author = "Tian Yu Fan"
title = "Turtles All The Way Down"
date = "2022-09-29"
description = "Localization, navigation, and pathplanning in unknown environments"
tags = []
categories = []
series = ["Unified Robotics"]
aliases = []
image = "Background.png"
+++

The main objective of this project was to implement algorithms for mobile robot mapping, localization, path planning, and navigation in an unknown environment.

# Tools
Software | Description
--------|------
Ubuntu 18.04 | Operating System
Robotics Operating System (ROS) | Software Framework
Python | Programming Language
Gazebo | Robotics Simulator
Rviz | Robotics Visualization
GMapping | LiDAR Mapping
AMCL | Localization

# Hardware
We used a TurtleBot3 Burger, a two-wheeled differential drive robot with an onboard Raspberry Pi and OpenCR control. The Turtlebot has a mounted 360 degrees LiDAR for point cloud mapping and encoders for odometry feedback.

![Turtlebot](http://robosklep.com/img/cms/turtlebot3_burger_components.png)

# Software
## Occupancy Grid
For successful exploration and navigation of an unknown environment, the Turtlebot scanned its environment using LiDAR to obtain point clouds.
OpenSlam's GMapping package is a laser-based SLAM (Simultaneous Localization and Mapping) that converts the point cloud scans into a 2D occupancy grid. Each cell in the occupancy grid contains the probability of its location being an obstacle. We defined a threshold for these probabilities, generating a map of drivable and undrivable areas. 

## Configuration Space
Expanding occupied regions reduced the occupancy grid's configuration space (C-space). This C-space introduces a buffer between the robot and the obstacles, minimizing the chances of collision. However, unavoidable collisions are inevitable because of unaccounted mechanical and sensor errors. 

## Frontiers and Centroids 
We traversed through this occupancy grid to find cells dividing the known and unknown regions, called frontier cells. Adjacent cells form frontiers, and the middle cell is the target centroid. The criteria for optimality depended on the frontier size and centroid distance from the robot's location. Among all calculated centroids, we chose the most optimal one. We deprioritize tiny and distant frontiers to minimize to economize time.  

## Localization
While mapping, we used the adjusted odometry readings from the GMapping package to determine the current estimate of the robot's pose within the map frame. Once the mapping phase is complete, the robot switches to the AMCL package for localization to alleviate the unnecessary overhead. The AMCL package uses the Monte Carlo localization algorithm, which iteratively scatters points across the map based on the current LiDAR readings and the expected features from the map. The algorithm iteratively redistributes the points in likely areas of the robot's location. In the best-case scenario, all the points converge in one place, successfully localizing the robot.  

## Path Planning
We planned from the robot's current location to the optimized centroid using A*. The graph was the occupancy grid that labeled cells as obstacles or traversable. The neighbor of a cell were its traversable and immediate orthogonal and diagonal neighbors. The primary heuristic was the euclidean distance between the start and end grid cells. The C-space cells carried increased weight such that regular paths never traverse these cells, but the TurtleBot can navigate out of this area if it must do so. Additionally, proximity to a wall increased the weight for collision avoidance. We commanded the robot to follow this generated path.

## Outcomes
We successfully mapped the entire unknown environment, returned to the starting location, and drove to a randomly specified place!