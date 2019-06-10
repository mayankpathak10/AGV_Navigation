[![Build Status](https://api.travis-ci.com/mayankpathak10/agv_navigation.svg?branch=master)](https://travis-ci.com/mayankpathak10/AGV_Navigation) [![Coverage Status](https://coveralls.io/repos/github/mayankpathak10/agv_navigation/badge.svg?branch=master)](https://coveralls.io/github/mayankpathak10/agv_navigation?branch=master) [![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](https://opensource.org/licenses/MIT)


## Overview:
The main idea of this project is to implement a path planning algorithm on an Automated Guided Vehicle (AGV) to plan its route from a start point to end point. An AGV is an advanced mobile robot that follows wires, markers, invisible UV markers, cameras and lasers to move. AGVs follow a defined path every time, they are not suitable for non-repetitive tasks and decrease the flexibility of operations overall.

We aim to demonstrate that the above-mentioned limitations of AGV can be alleviated by implementing a novel path planning technique (RRT algorithm) for searching the path in a known environment. To get the information about the environment, this method is topped by a g-mapping technique using SLAM and develop a binary map of the environment to feed to the RRT algorithm.

The AGV (TurtleBot in our project) drives around in a simulated world, navigating through obstacles using LIDAR sensor. Whenever an obstacle is recognized, the g-mapping algorithm prepares a binary 2D map of the environment. For example, if the robot finds an obstacle while moving around, like a wall or any solid surface, it refers that coordinates as 1 , meaning that there is a obstacle in front of the robot.

After preparing a map of the environment, the robot uses path planning algorithm (RRT algorithm) and finds a optimal path between its current position and the goal point.

The presentation for the project can be found on Youtube [here](https://youtu.be/zoL4Fnlx-bw). The video provides an overview of the project, UML diagrams, as well as a demo.

## About Authors
__Bhargav Dandamudi:__ Graduate Student in M.Eng., Robotics, working as Teaching Assitant of Grad. Level Robotics Course. I am intrested to pursue career in Computer vision, planning and Medical Robotics related field.

__Mayank Pathak:__ Graduate Student in M.Eng., Robotics, working as Research Assistant in Sensors and Actuators Lab in the University of Maryland, College Park. I am interested in pursue career in the field of Computer Vision and Machine Learning.

## Project Approach:
1. Develop a custom world in the gazebo.
	A map containing walls and other obstacles representing industrial workshop scenario is prepared.
2. A 2D map is prepared using the laserscan, position and pose information of turtlebot from gazebo world and move the turtlebot around in this unknown map to avoid obstacles and generate 2D map.
3. Using this map, a path between two given points (Start point and Destination) is calculated using Rapidly Exploring Random Tree (RRT) path planning algorithm.
4. RRT algorithm is implemented in its simplicity to find the next closest vertex near the randomly generated point and move to that point, repeat this and gradually reach the goal point.
5. TurtleBot navigates, avoiding obstacles and reaches the destination.
	After the path is known, turtlebot navigates in the simulated gazebo world using the node coordinates.
6. Quality is assured by delivering Unit Tests for overall source code and full coverage in coveralls.

The UML diagrams for the project are as follows:
### UML Activity Diagram
![activity_diag](https://github.com/mayankpathak10/AGV_Navigation/blob/master/images/agv_navigation_ActivityDiagram.jpeg)
### UML Class Diagrams
![mapping_class_diag](https://github.com/mayankpathak10/agv_navigation/blob/master/images/Revision_2ActivityDiag.jpeg)
![planning_class_diag](https://github.com/mayankpathak10/agv_navigation/blob/master/images/Revision2_agv_navigation.jpeg)

## Deveopment Using Solo Iterative Process (SIP) and Test-Driven Development (TDD)
In development of this software module, along with test-driven development(TDD), solo interative process (SIP) was followed. Using SIP this software module is divided into two parts: the product backlog, and code of the software.

First, the product backlog was developed as per the requirements. From these entries, highest priority requirements were selected for first-week sprint. In the product backlog, estimated time of completion was allotted to every task. Actual completion time was compared with estimated completion time and based on that, time allotment for future tasks was modified.

After the planning was done, TDD process was used in this which UML activity and class diagrams were developed according to the requirements of the project. Based on UML class diagram, unit tests will written. Then stub classes were written with functions matching the test cases.

Following is the link to the spreadsheet that contains detailed entries of the product backlog, time log, error log and release backlog: [Product Backlog](https://docs.google.com/spreadsheets/d/1xaJ7oK8ZxaKgJi8yriTa_Gf3EtpvXDz0xDdI2Yvn0hE/edit?usp=sharing)

## Sprint Planning
This project development is divided into 3 weekly sprints. Detailed Sprint planning can be found in the below file:
[Sprint Planning Notes](https://docs.google.com/document/d/1IQnlFH5AhvnU3X79OxwiI05bf_QO1nF9Tf4WWig5hUM/edit?usp=sharing)

## Dependencies
- Ubuntu 16.04
- ROS Kinetic
- Catkin
- Gazebo
- Turtlebot Gazebo package
- Packages included in ROS Kinetic:
    - roscpp
	- std_msgs
	- genmsg
	- geometry_msgs
	- gmapping
	- Gmapping
	- map_server


## Custom Gazebo World
A Gazebo simulation world of dimensions 10m x 10m (length x width) is prepared to implement this project.

![gazebo](https://github.com/mayankpathak10/agv_navigation/blob/master/images/gazebo.jpg)

## How to build
If you do not have a catkin workspace, in a new terminal:
```
mkdir -p ~/catkin_ws/src
cd ~/catkin_ws/
catkin_make
source devel/setup.bash
cd src/
git clone https://github.com/mayankpathak10/agv_navigation
cd ..
catkin_make
```
If you wish to run this code in an existing catkin workspace:
```
cd ~/catkin_ws/
source devel/setup.bash
cd src/
git clone https://github.com/mayankpathak10/agv_navigation
cd ..
catkin_make
```

## Demo
Firstly we make map of the custom environment using agv_navigator node, then to move around the map autonomously we need to run turtlebot_rrt planner.
After following the build instructions:

To run the demo, in a new terminal:
```
cd ~/catkin_ws/
source devel/setup.bash
roslaunch agv_navigator agvnavigator.launch
```
This will load the turtlebot in the gazebo world and wait for 15 seconds. Now to run gmapping and Rviz, in a new terminal:
```
cd ~/catkin_ws/
source devel/setup.bash
roslaunch agv_navigation demo.launch
```
This will open rviz to visualize the map in realtime as it updates its location and surroundings. One can also watch the agv move around in gazebo map as it maps the environment.
### Saving the Map
 To save the map as created by the rviz, in a new terminal:
```
rosrun map_server map_saver -f <map_name>
```
To view the saved map. In a new terminal
```
eog <map_name>.pgm
```
Developed Custom world's Saved Map is as shown below
![Rviz_Map](https://github.com/mayankpathak10/agv_navigation/blob/master/images/Rviz_Map.png)

Once you have created your map place the ```.world```, ```.yaml```, and ```.pgm``` file into the ```/maps```  directory all using the same file name and then use the ```map_name``` arg with the launch file:

```
roslaunch agv_navigation rrt_planner.launch
```
![acme_floor_gazebo_rviz](https://github.com/mayankpathak10/agv_navigation/blob/master/images/acme_floor_gazebo_rviz.png)
### Launch file args
```map_name``` indicates the map that you want to use. This assumes that you already have a ```.world```, ```.yaml```, and a ```.pgm``` file in the ```/maps``` directory.

```step_size``` indicates the size of the step that the RRT algorithm uses. Sometimes called epsilon. A lower value is more accurate, a higher value is faster. Parameter is entered in meters. Has a default value of 2.5.

```max_iterations``` determines the maximum number of vertices that will be created searching for a path between the starting point and the goal. This prevents infinite loops in the event the goal is unreachable. Parameter is an integer. Has a default value of 200,000.

```delta``` is the incremental amount that will be checked along each step_size for obstacles. Choosing a lower value is more accurate, a higher value is faster. Parameter is entered in meters. Has a default value of 1.0.

```goal_radius``` is how close you have to be to the goal for it to count. Parameter is entered in meters. Has a default value of 1.0.


This will launch rviz and gazebo simultaneously showing turtlebot ready to navigate autonomously.
To set a goal for the robot switch to the Rviz window and and click "2D Nav Goal" at the top and select on the map where you want the robot to go.


## Running Rostest
To run rostest, in a new terminal:
```

cd ~/catkin_ws/
source devel/setup.bash
```
Open second terminal and run
```
roscore
```
In the first terminal run the following
```
cd build/
make run_tests
```


#### Code coverage
To see the code coverage execute the following command in the build folder of your carkin workspace. Make sure you have `catkin_make` and `make run_tests` earlier

```
lcov --directory agv_navigation/CMakeFiles/agv_navigation_node.dir/src/ --capture --output-file coverage.info
```
To Read the generated coverage file, execute
`lcov --list coverage.info`

To generate a HTML file for better viewing and understanding the lcov code coverage, execute the following command
`genhtml coverage.info --output-directory out`

This will create a folder named `out` in the current directory.
Open `out/index.html` to view the code coverage details.


## Doxygen Documentation
To generate Doxygen Documentation in HTML and LaTEX, follow the next steps:
```
cd <path to repository>
mkdir <documentation_folder_name>
cd <documentation_folder_name>
doxygen -g <config_file_name>
```
Inside the configuration file, update:
```
PROJECT_NAME = 'project name'
INPUT = ../agv_navigation ../include ../test
```
Doxygen files will be generated to /docs folder

To view them in a browser:
```
cd docs
cd html
firefox index.html
```
## Known issues and Bugs
* When gazebo initializes it  throws some errors of missing plugins and thi followin error:
  SpawnModel: Failure - model name mobile_base already exist.
* Visualization markers not shown by Rviz


## Recording a Bag file
```
cd ~/catkin_ws/
source devel/setup.bash
roslaunch agv_navigation agvnav.launch rec:=1
```
This will save a bag file in the results folder of the package.

To inspect the bag file, In a new terminal:
```
cd ~/catkin_ws/
source devel/setup.bash
cd src/agv_navigation/Results
rosbag info recording.bag
```

To replay the bag file,
The bag file inside the Results folder was compressed due to size issues, to run it, follow:
(Git only accepts 100MB files)
```
cd ~/catkin_ws/
cd src/agv_navigation/Results
rosbag decompress -f recording.bag
```


first run rosmaster from a terminal:
```
roscore
```
Now, from the results folder run the following command in a new terminal:
```
cd ~/catkin_ws/
source devel/setup.bash
cd src/agv_navigation/Results
rosbag play recording.bag
```

## License
MIT License

Copyright (c) 2018 Mayank Pathak, Bhargav Dandamudi

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the software is furnished to do so, subject to the following conditions:
 The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.
 THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
