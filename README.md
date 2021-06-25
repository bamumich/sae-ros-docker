### sae-ros-docker

| Author | Email | Date |
| :----: | :----: | :----: |
| Brian A. Mulrooney | bamu@umich.edu | 2021.06.24 |


- [x] Dockerfile generated and tested successfully, overcame catkin hurdle.
	
	`
	$ RUN /bin/bash -c '. /opt/ros/noetic/setup.bash cd $ROS_WS/src catkin_make'
	`
- [x] Optimus Manager: dGPU Nvidia

	`
	$ optmus-manager --switch hybrid -y
	`
- [x] TurtleBot3 Confirmed Working
- [x] Gazebo Confirmed Working
- [x] F1/10 Simulator Confirmed Working
#### Useful docker commands:

##### Example of Building from Dockerfile
	`
	$ docker build -t sae_ws:ros_ws .
	
##### Example of mounting the Lane Keeping repo into docker	
	$ docker run -it --rm \
		-v /home/bamu/repo/bootcamp/Lane_Keeping:/opt/ros_ws/src/Lane_Keeping \
 		--env="QT_X11_NO_MITSHM=1" \
 		-v /tmp/.X11-unix:/tmp/.X11-unix \
 		-e DISPLAY=unix$DISPLAY \
 		sae_ws:ros_ws

##### Example of mounting the Wall Following repo into docker
	$ docker run -it -d --rm \
		-v /home/bamu/repo/bootcamp/Wall_following:/opt/ros_ws/src/Wall_following \
 		--env="QT_X11_NO_MITSHM=1" \
 		-v /tmp/.X11-unix:/tmp/.X11-unix \
 		-e DISPLAY=unix$DISPLAY \
 		sae_ws:ros_ws

##### Example of Viewing currently running containers, and entry point
	$ docker ps
	$ docker exec -it $CONTAINER_ID bash
	$ docker exec -it 89c1429ad42b bash
	`
##### Launching ROS, Turtlebot, F1/10 Simulator
	`
	$ roscore
    $ roslaunch turtlebot3_gazebo turtlebot3_empty_world.launch
	$ roslaunch race f1_tenth.launch
	$ roslaunch f1tenth_simulator simulator.launch
	$ roslaunch lane_keeping lane_keep.launch
	`

##### [ROS Docker Tutorial](https://wiki.ros.org/docker/Tutorials/GUI)

##### [OSRF Rocker](https://github.com/osrf/rocker)
