FROM althack/ros2:humble-dev

# ** [Optional] Uncomment this section to install additional packages. **
#
ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update \
 && apt-get -y install --no-install-recommends python3-pip \
 && apt-get -y install --no-install-recommends alsa-utils \
 && apt-get -y install --no-install-recommends python3-colcon-argcomplete \
 && apt-get -y install --no-install-recommends ros-${ROS_DISTRO}-cartographer-* \
 && apt-get -y install --no-install-recommends ros-${ROS_DISTRO}-desktop-* \
 && apt-get -y install --no-install-recommends ros-${ROS_DISTRO}-joint-state-publisher-* \
 && apt-get -y install --no-install-recommends ros-${ROS_DISTRO}-nav2-* \
 && apt-get -y install --no-install-recommends ros-${ROS_DISTRO}-rosbridge-* \
 && apt-get -y install --no-install-recommends ros-${ROS_DISTRO}-rqt-* \
 && apt-get -y install --no-install-recommends ros-${ROS_DISTRO}-rtabmap-* \
 && apt-get -y install --no-install-recommends ros-${ROS_DISTRO}-usb-cam \
 && apt-get -y install --no-install-recommends ros-${ROS_DISTRO}-depthai-* \
 && apt-get -y install --no-install-recommends gazebo \
 && apt-get -y install --no-install-recommends ros-${ROS_DISTRO}-gazebo-* \
#    # Clean up
   && apt-get autoremove -y \
   && apt-get clean -y \
   && rm -rf /var/lib/apt/lists/*
ENV DEBIAN_FRONTEND=dialog

ARG USERNAME
RUN usermod -aG dialout ${USERNAME}
RUN usermod -aG video ${USERNAME}

# Set up auto-source of workspace for ros user
ARG WORKSPACE
RUN echo "if [ -f ${WORKSPACE}/install/setup.bash ]; then source ${WORKSPACE}/install/setup.bash; fi" >> /home/ros/.bashrc
