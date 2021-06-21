# Install Ubuntu 20.04 LTS - Focal Fossa
FROM ubuntu:20.04

# Handle user-prompt for Ubuntu installation time zone selection
ENV TZ=America
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN apt-get update && apt-get upgrade -y && apt-get install -y apt-utils

# Install dependencies for ROS - Noetic
RUN apt-get update --fix-missing && apt-get upgrade -y && apt-get install -y \
        sudo dialog git openssh-server wget curl cmake nano python3 python3-pip \
        python3-empy python3-nose python3-setuptools build-essential \
        libgtest-dev libsdl2-dev libsdl2-2.0-0 libsdl2-image-dev libsdl2-image-2.0-0 \
        libboost-all-dev

RUN apt-get autoclean && apt-get autoremove && apt-get update --fix-missing && \
        apt-get upgrade -y && dpkg --configure -a && apt-get install -f

# Install required python3 packages
RUN pip3 install glob2 regex

# Handle user-prompt for debconf frontend keyboard selection
RUN echo 'debconf debconf/frontend select Noninteractive' | sudo debconf-set-selections

# Install ROS - Noetic & Packages
RUN sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" \
        > /etc/apt/sources.list.d/ros-latest.list'

RUN apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' \
        --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654

RUN apt-get update --fix-missing && apt-get upgrade -y && apt-get install -y \
        ros-noetic-ros-base ros-noetic-catkin \
        ros-noetic-joy ros-noetic-teleop-twist-joy \
        ros-noetic-teleop-twist-keyboard ros-noetic-laser-proc \
        ros-noetic-rgbd-launch ros-noetic-rosserial-arduino \
        ros-noetic-rosserial-python ros-noetic-rosserial-client \
        ros-noetic-rosserial-msgs ros-noetic-amcl ros-noetic-map-server \
        ros-noetic-move-base ros-noetic-urdf ros-noetic-xacro \
        ros-noetic-compressed-image-transport ros-noetic-rqt* ros-noetic-rviz \
        ros-noetic-gmapping ros-noetic-navigation ros-noetic-interactive-markers \
        ros-noetic-dynamixel-sdk ros-noetic-turtlebot3-msgs ros-noetic-turtlebot3 \
        ros-noetic-pid ros-noetic-turtlesim \
        python3-rosdep python3-rosinstall python3-rosinstall-generator python3-wstool \
        xserver-xorg xinit mesa-utils libegl1-mesa libegl1-mesa-dev \
        libgbm-dev libgbm1 libgl1-mesa-dev libgl1-mesa-dri libgl1-mesa-glx \
        libglu1-mesa libglu1-mesa-dev mesa-common-dev qt5-default\
        libgazebo11-dev \
        ros-noetic-ros-control ros-noetic-gazebo-ros-control \
        ros-noetic-ros-controllers ros-noetic-teb-local-planner \
        && rm -rf /var/lib/apt/lists/*

# Bash hoops to jump through
SHELL ["/bin/bash", "-c"]
RUN source /opt/ros/noetic/setup.bash
RUN echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc
RUN source ~/.bashrc

ENV ROS_WS /opt/ros_ws
RUN mkdir -p $ROS_WS/src
RUN /bin/bash -c '. /opt/ros/noetic/setup.bash; catkin_init_workspace $ROS_WS/src'
WORKDIR $ROS_WS
RUN /bin/bash -c '. /opt/ros/noetic/setup.bash; cd $ROS_WS; catkin_make'
RUN echo "source $ROS_WS/devel/setup.bash" >> ~/.bashrc
RUN source ~/.bashrc

RUN rosdep init
RUN rosdep update

# Download & Install ROS Tutorials, TurtleBot, & F1 Tenth
WORKDIR $ROS_WS
RUN git -C src clone \
      -b noetic-devel \
      https://github.com/ros/ros_tutorials.git
RUN git -C src clone \
        -b noetic-devel \
        https://github.com/ROBOTIS-GIT/turtlebot3_simulations.git
# RUN git -C src clone \
#        -b noetic-devel \
#        https://github.com/linklab-uva/f1tenth_gtc_tutorial

# Build ROS Package Source
RUN /bin/bash -c '. /opt/ros/noetic/setup.bash cd $ROS_WS rosmake turtlesim'
RUN /bin/bash -c '. /opt/ros/noetic/setup.bash cd $ROS_WS rosmake turtlebot3'
RUN /bin/bash -c '. /opt/ros/noetic/setup.bash cd $ROS_WS catkin_make'

WORKDIR $~
RUN echo "export TURTLEBOT3_MODEL=burger" >> ~/.bashrc
RUN source ~/.bashrc

WORKDIR $ROS_WS
RUN /bin/bash -c '. /opt/ros/noetic/setup.bash cd $ROS_WS catkin_make'