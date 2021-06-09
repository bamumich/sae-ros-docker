# sae-ros-docker

Author:	 Brian A. Mulrooney
Email:	 bamu@umich.edu
Purpose: ROS1 Docker Container for SAE ADAS Bootcamp & UMDearborn
Goal: 	 ROS2 Docker Container for SAE ADAS Bootcamp & UMDearborn

--------------
: 2021.06.08 :
--------------

* Dockerfile generated and tested successfully, overcame catkin hurdle.
	# RUN /bin/bash -c '. /opt/ros/noetic/setup.bash cd $ROS_WS/src catkin_make'
* Useful docker commands:
	# docker build -t sae_ws:ros_ws . 
	# docker run -d -it -t sae_ws:ros_ws
	# docker ps
	# docker exec -it $CONTAINER_ID bash
	# docker exec -it 84845b3c0d9d bash
	# docker exec -it 647912f5efa2 bash
	# docker exec -it 8a820f253888 bash

	# docker run -d -it --rm --env="DISPLAY" -v "$HOME/.Xauthority:/root/.Xauthority:rw" --net=host sae_ws:ros_ws

	# docker run -v /tmp/.X11-unix:/tmp/.X11-unix -e DISPLAY=unix$DISPLAY sae_ws:ros_ws