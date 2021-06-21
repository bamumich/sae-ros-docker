### sae-ros-docker

| Author | Email | Date |
| :----: | :----: | :----: |
| Brian A. Mulrooney | bamu@umich.edu | 2021.06.21 |


- Dockerfile generated and tested successfully, overcame catkin hurdle.
	
	<p> $ RUN /bin/bash -c '. /opt/ros/noetic/setup.bash cd $ROS_WS/src catkin_make'</p>

- Useful docker commands:
	<p>$ docker build -t sae_ws:ros_ws .</p>
	<p>$ docker run -it --rm \</p>
 		<p>--env="QT_X11_NO_MITSHM=1" \</p>
 		<p>-v /tmp/.X11-unix:/tmp/.X11-unix \</p>
 		<p>-e DISPLAY=unix$DISPLAY \</p>
 		<p>sae_ws:ros_ws</p>
	<p>$ docker ps</p>
	<p>$ docker exec -it $CONTAINER_ID bash</p>
	<p>$ docker exec -it ad633723cabd bash</p>

- Launching ROS & Turtlebot
	<p>$ roscore</p>

    <p>$ roslaunch turtlebot3_gazebo turtlebot3_empty_world.launch</p>

- Optimus Manager: dGPU Nvidia
	<p>$ optmus-manager --switch hybrid -y</p>
- ROS Docker Tutorial:
https://wiki.ros.org/docker/Tutorials/GUI

- OSRF Rocker
https://github.com/osrf/rocker
