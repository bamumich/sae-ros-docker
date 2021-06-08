# docker build -t sae_ws:ros_ws .

# Install Ubuntu 20.04 LTS - Focal Fossa
FROM ubuntu:20.04

# Handle user-prompt for Ubuntu installation time zone selection
ENV TZ=America
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN apt-get update && apt-get upgrade -y && apt-get install -y apt-utils

# Install dependencies for ROS - Noetic
RUN apt-get update --fix-missing && apt-get upgrade -y && apt-get install -y \
        sudo dialog git openssh-server wget curl cmake python3 python3-pip \
        python3-empy python3-nose python3-setuptools build-essential \
        libgtest-dev libsdl2-dev libsdl2-2.0-0 libsdl2-image-dev libsdl2-image-2.0-0 \
        libboost-all-dev

RUN apt-get autoclean && apt-get autoremove && apt-get update --fix-missing && \
        apt-get upgrade -y && dpkg --configure -a && apt-get install -f

# Install required python3 packages
RUN pip3 install glob2 regex

# Handle user-prompt for debconf frontend keyboard selection
RUN echo 'debconf debconf/frontend select Noninteractive' | sudo debconf-set-selections

RUN sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" \
        > /etc/apt/sources.list.d/ros-latest.list'

RUN apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' \
        --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654

RUN apt-get update --fix-missing && apt-get upgrade -y && apt-get install -y \
        ros-noetic-ros-base \
        ros-noetic-joy ros-noetic-teleop-twist-joy \
        ros-noetic-teleop-twist-keyboard ros-noetic-laser-proc \
        ros-noetic-rgbd-launch ros-noetic-rosserial-arduino \
        ros-noetic-rosserial-python ros-noetic-rosserial-client \
        ros-noetic-rosserial-msgs ros-noetic-amcl ros-noetic-map-server \
        ros-noetic-move-base ros-noetic-urdf ros-noetic-xacro \
        ros-noetic-compressed-image-transport ros-noetic-rqt* ros-noetic-rviz \
        ros-noetic-gmapping ros-noetic-navigation ros-noetic-interactive-markers \
        ros-noetic-dynamixel-sdk ros-noetic-turtlebot3-msgs ros-noetic-turtlebot3 \
        ros-noetic-pid ros-noetic-catkin