### sae-ros-docker

| Author | Email | Date |
| :----: | :----: | :----: |
| Brian A. Mulrooney | bamu@umich.edu | 2021.06.25 |


- [x] Dockerfile generated and tested successfully, overcame catkin hurdle.
	$ RUN /bin/bash -c '. /opt/ros/noetic/setup.bash cd $ROS_WS/src catkin_make'
- [x] Optimus Manager: dGPU Nvidia
	$ optmus-manager --switch hybrid -y
- [x] TurtleBot3 Confirmed Working
- [x] Gazebo Confirmed Working
- [x] F1/10 Simulator Confirmed Working
- [x] TurtleBot3 OpenCV
- [_] Tensorflow
- [x] Final Project
#### Useful docker commands:

##### Example of Building from Dockerfile
	$ docker build -t bootcamp .
	
##### Example of mounting the Lane Keeping repo into docker	
	$ docker run -it --rm \
		-v /home/bamu/repo/bootcamp/lane-keeping:/opt/ros_ws/src/lane-keeping \
 		--env="QT_X11_NO_MITSHM=1" \
 		-v /tmp/.X11-unix:/tmp/.X11-unix \
 		-e DISPLAY=unix$DISPLAY \
 		bootcamp

##### Example of mounting the Wall Following repo into docker
	$ docker run -it --rm \
		-v /home/bamu/repo/bootcamp/wall-following:/opt/ros_ws/src/wall-following \
 		--env="QT_X11_NO_MITSHM=1" \
 		-v /tmp/.X11-unix:/tmp/.X11-unix \
 		-e DISPLAY=unix$DISPLAY \
 		bootcamp

##### Example of mounting the Pure Pursuit repo into docker
	$ docker run -it --rm \
		-v /home/bamu/repo/bootcamp/pure_pursuit:/opt/ros_ws/src/pure_pursuit \
		-v /opt/sae_ws/urdf:/opt/ros/noetic/share/turtlebot3_description/ \
 		--env="QT_X11_NO_MITSHM=1" \
 		-v /tmp/.X11-unix:/tmp/.X11-unix \
 		-e DISPLAY=unix$DISPLAY \
 		bootcamp

##### Example of mounting the TB3 OpenCV repo into docker
	$ docker run -it --rm \
		-v /home/bamu/repo/bootcamp/tb3_opencv/:/opt/ros_ws/src/tb3_opencv \
		-v /opt/sae_ws/urdf:/opt/ros/noetic/share/turtlebot3_description/urdf \
 		--env="QT_X11_NO_MITSHM=1" \
 		-v /tmp/.X11-unix:/tmp/.X11-unix \
 		-e DISPLAY=unix$DISPLAY \
 		bootcamp

##### Example of mounting the Final Project repo into docker
	$ docker run -it --rm \
		-v /home/bamu/repo/bootcamp/Final_Project/:/opt/ros_ws/src/aue_finals \
		-v /opt/sae_ws/urdf:/opt/ros/noetic/share/turtlebot3_description/urdf \
 		--env="QT_X11_NO_MITSHM=1" \
 		-v /tmp/.X11-unix:/tmp/.X11-unix \
 		-e DISPLAY=unix$DISPLAY \
 		bootcamp

##### Example of Viewing currently running containers, and entry point
	$ docker ps
	$ docker exec -it $CONTAINER_ID bash
	$ docker exec -it 523f307c4e76 bash
	
##### Launching ROS, Turtlebot, F1/10 Simulator
	$ roscore
    $ roslaunch turtlebot3_gazebo turtlebot3_empty_world.launch
	$ roslaunch race f1_tenth.launch
	$ roslaunch f1tenth_simulator simulator.launch
	$ roslaunch lane_keeping lane_keep.launch
	$ roslaunch wall_following wall_following.launch control:=0
	$ roslaunch wall_following wall_following.launch control:=1
	$ roslaunch tb3_opencv turtlebot3_follow_line.launch
	$ roslaunch aue_finals turtlebot3_autonomy_final.launch


##### [ROS Docker Tutorial](https://wiki.ros.org/docker/Tutorials/GUI)

##### [OSRF Rocker](https://github.com/osrf/rocker)
