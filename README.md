### sae-ros-docker

| Author | Email | Date |
| :----: | :----: | :----: |
| Brian A. Mulrooney | bamu@umich.edu | 2021.06.21 |


#### Dockerfile generated and tested successfully, overcame catkin hurdle.
	
	`$ RUN /bin/bash -c '. /opt/ros/noetic/setup.bash cd $ROS_WS/src catkin_make'`

#### Useful docker commands:
	`$ docker build -t sae_ws:ros_ws .`
	`$ docker run -it --rm \
 		--env="QT_X11_NO_MITSHM=1" \
 		-v /tmp/.X11-unix:/tmp/.X11-unix \
 		-e DISPLAY=unix$DISPLAY \
 		sae_ws:ros_ws
	`$ docker ps</p>
	`$ docker exec -it $CONTAINER_ID bash`
	`$ docker exec -it ad633723cabd bash`

#### Launching ROS & Turtlebot
	`$ roscore`
    `$ roslaunch turtlebot3_gazebo turtlebot3_empty_world.launch`

#### Optimus Manager: dGPU Nvidia
	`$ optmus-manager --switch hybrid -y`
#### [ROS Docker Tutorial]
(https://wiki.ros.org/docker/Tutorials/GUI)

#### [OSRF Rocker]
(https://github.com/osrf/rocker)
