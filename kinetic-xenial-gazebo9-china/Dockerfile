FROM nvidia/opengl:1.1-glvnd-runtime-ubuntu16.04

# config
ENV USER=rosuser
ENV PASSWD=rosuser
ENV UID=1000
ENV GID=1000

# preflight
RUN sed -i 's/http:\/\/.*\ubuntu/http:\/\/mirrors.tuna.tsinghua.edu.cn\/ubuntu/g' /etc/apt/sources.list && \ 
    apt-get update && apt-get install -y \
    mesa-utils wget sudo apt-utils curl vim virtualenv

# install ros
RUN sh -c 'echo "deb http://mirrors.tuna.tsinghua.edu.cn/ros/ubuntu/ xenial main" > /etc/apt/sources.list.d/ros-latest.list' && \
    apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654 && \
    apt-get update && apt-get install -y \ 
    ros-kinetic-desktop-full python-rosinstall python-rosinstall-generator python-wstool build-essential

# install gazebo
RUN sh -c 'echo "deb http://packages.osrfoundation.org/gazebo/ubuntu-stable xenial main" > /etc/apt/sources.list.d/gazebo-stable.list' && \
    wget http://packages.osrfoundation.org/gazebo.key -O - | sudo apt-key add - && \
    apt-get update && \
    apt-get install -y \
    gazebo9 libgazebo9-dev ros-kinetic-gazebo9-ros-pkgs ros-kinetic-gazebo9-ros-control

# postinatall: add new user & clean up
RUN useradd --create-home -m $USER && \
    echo "$USER:$PASSWD" | chpasswd && \
    usermod --shell /bin/bash $USER && \
    usermod -aG sudo $USER && \
    echo "$USER ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers && \
    usermod --uid $UID $USER && \
    groupmod --gid $GID $USER && \
    rm -rf /var/lib/apt/lists/* && \
    rosdep init

# setup ros
USER $USER
RUN rosdep update && \
    sh -c 'echo "source /opt/ros/kinetic/setup.bash" >> ~/.bashrc'

# nvidia-container-runtime
ENV NVIDIA_VISIBLE_DEVICES \
    ${NVIDIA_VISIBLE_DEVICES:-all}
ENV NVIDIA_DRIVER_CAPABILITIES \
    ${NVIDIA_DRIVER_CAPABILITIES:+$NVIDIA_DRIVER_CAPABILITIES,}graphics