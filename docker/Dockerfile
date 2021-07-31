# Install Ubuntu 20.04 LTS - Focal Fossa
FROM nvidia/cuda:11.3.1-runtime-ubuntu20.04

# Handle user-prompt for Ubuntu installation (time zone selection & keyboard)
ENV DEBIAN_FRONTEND=noninteractive
ENV TZ=America
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN apt-get update && apt-get upgrade -y && apt-get install -y apt-utils

# Install dependencies for ROS - Noetic
RUN apt-get update --fix-missing && apt-get upgrade -y && apt-get install -y \
        sudo dialog git openssh-server wget curl cmake nano python3 python3-pip \
        python3-empy python3-nose python3-setuptools build-essential gedit \
        libgtest-dev libsdl2-dev libsdl2-2.0-0 libsdl2-image-dev libsdl2-image-2.0-0 \
        libboost-all-dev libgbm-dev libgbm1 libgl1-mesa-dev libgl1-mesa-dri \
        libgl1-mesa-glx libglu1-mesa libglu1-mesa-dev mesa-common-dev \
        xserver-xorg xinit mesa-utils libegl1-mesa  \
        qt5-default libopencv-dev python3-opencv software-properties-common

# Install QT4
RUN add-apt-repository ppa:gezakovacs/ppa
RUN apt-get update --fix-missing && apt-get upgrade -y && apt-get install -y \
        qt4-default

# Clean & Fix Any Conflicts
RUN apt-get autoclean && apt-get autoremove -y && apt-get update --fix-missing && \
        apt-get upgrade -y && dpkg --configure -a && apt-get install -f

# Install required python3 packages
RUN pip3 install glob2 regex pyqtgraph 

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
        ros-noetic-ros-control ros-noetic-gazebo-ros-control ros-noetic-ros-controllers \
        ros-noetic-teb-local-planner libgazebo11-dev ros-noetic-urg-c \
        ros-noetic-ackermann-msgs ros-noetic-gazebo-ros-pkgs \
        ros-noetic-tf2-geometry-msgs ros-noetic-driver-base \
        ros-noetic-tf-conversions ros-noetic-usb-cam \
        ros-noetic-image-transport ros-noetic-cv-bridge ros-noetic-vision-opencv \
        python3-opencv libopencv-dev ros-noetic-image-proc \
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

# Init & Update ROSdep
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

RUN git -C src clone \
        https://github.com/bamumich/sae-f1-sim.git

RUN git -C src clone \
        https://github.com/bamumich/aue_finals

WORKDIR /opt/ros_ws/src/
RUN mv sae-f1-sim f110-simulator-public

# Build ROS Package Source
RUN /bin/bash -c '. /opt/ros/noetic/setup.bash cd $ROS_WS rosmake turtlesim'
RUN /bin/bash -c '. /opt/ros/noetic/setup.bash cd $ROS_WS rosmake turtlebot3'
RUN /bin/bash -c '. /opt/ros/noetic/setup.bash cd $ROS_WS rosmake aue_finals'
RUN /bin/bash -c '. /opt/ros/noetic/setup.bash cd $ROS_WS catkin_make'

WORKDIR $~
RUN echo "export TURTLEBOT3_MODEL=burger" >> ~/.bashrc
RUN source ~/.bashrc

ENV GAZEBO_MODELS_PATH="/opt/ros_ws/src/turtlebot3_simulations/turtlebot3_gazebo/models"
RUN cp -r /opt/ros_ws/src/aue_finals/models/. $GAZEBO_MODELS_PATH
RUN echo "export GAZEBO_MODEL_PATH=$GAZEBO_MODELS_PATH" >> ~/.bashrc
RUN source ~/.bashrc

WORKDIR $ROS_WS
RUN /bin/bash -c '. /opt/ros/noetic/setup.bash; cd $ROS_WS; catkin_make'
RUN echo "source $ROS_WS/devel/setup.bash" >> ~/.bashrc
RUN source ~/.bashrc

WORKDIR $ROS_WS
RUN /bin/bash -c '. /opt/ros/noetic/setup.bash cd $ROS_WS catkin_make'

# Install Tensorflow
# WORKDIR /

# RUN pip3 install launchpadlib keras==2.4.3 h5py numpy opencv-python
# RUN pip3 install --user --upgrade tensorflow==2.4.1


# RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
# LABEL maintainer "NVIDIA CORPORATION <cudatools@nvidia.com>"
# ENV NCCL_VERSION 2.9.9

# RUN apt-get update --fix-missing && apt-get upgrade -y && apt-get install -y \
#         pciutils nvidia-cuda-toolkit \
#         libtinfo5 libncursesw5 \
#         cuda-cudart-dev-11-3=11.3.109-1 \
#         cuda-command-line-tools-11-3=11.3.1-1 \
#         cuda-minimal-build-11-3=11.3.1-1 \
#         cuda-libraries-dev-11-3=11.3.1-1 \
#         cuda-nvml-dev-11-3=11.3.58-1 \
#         libnpp-dev-11-3=11.3.3.95-1 \
#         libnccl-dev=2.9.9-1+cuda11.3 \
#         libcublas-dev-11-3=11.5.1.109-1 \
#         libcusparse-dev-11-3=11.6.0.109-1 \
#         && rm -rf /var/lib/apt/lists/*

# # apt from auto upgrading the cublas package.
# # See https://gitlab.com/nvidia/container-images/cuda/-/issues/88
# RUN apt-mark hold libcublas-dev-11-3 libnccl-dev
# ENV LIBRARY_PATH /usr/local/cuda/lib64/stubs

# ENV CUDNN_VERSION 8.2.1.32
# ENV CUDA_VERSION cuda11.3
# LABEL com.nvidia.cudnn.version="${CUDNN_VERSION}"

# RUN apt-get update && apt-get install -y --no-install-recommends \
#     libcudnn8=$CUDNN_VERSION-1+cuda11.3 \
# && \
#     apt-mark hold libcudnn8 && \
#     rm -rf /var/lib/apt/lists/*

# # import os; import tensorflow as tf
# # os.environ['CUDA_VISIBLE_DEVICES'] ="0"
# # os.environ['TF_XLA_FLAGS'] = '--tf_xla_enable_xla_devices'
# # print("GPUs: ", len(tf.config.experimental.list_physical_devices('GPU')))
# # print(tf.reduce_sum(tf.random.normal([1000, 1000])))

# # lspci | grep -i nvidia
# # uname -m && cat /etc/*release

# # docker run -it --rm --gpus all test:tf

# RUN add-apt-repository ppa:graphics-drivers/ppa
# RUN apt-get install --no-install-recommends nvidia-driver-470 -y
