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
	# docker run -d -it --rm -v /tmp/.X11-unix:/tmp/.X11-unix -e DISPLAY=unix$DISPLAY sae_ws:ros_ws
	# docker ps
	# docker exec -it $CONTAINER_ID bash
	# docker exec -it e44cfd557a21 bash