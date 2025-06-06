FROM ubuntu:focal

# Non-interactive installation
ENV DEBIAN_FRONTEND=noninteractive
RUN echo 'debconf debconf/frontend select Noninteractive' | debconf-set-selections

# Timezone
RUN echo 'Etc/UTC' > /etc/timezone && \
    ln -sf /usr/share/zoneinfo/Etc/UTC /etc/localtime && \
    apt-get update && \
    apt-get install -y tzdata && \
    rm -rf /var/lib/apt/lists/*

# Install base tools
RUN apt-get update && apt-get install -y --no-install-recommends \
    curl \
    gnupg \
    lsb-release \
    ca-certificates \
    build-essential \
    git \
    python3-pip \
    python3-rosdep \
    && rm -rf /var/lib/apt/lists/*

# Setup ROS keyrings directory
RUN mkdir -p /etc/apt/keyrings

# Download and save ROS GPG key
RUN curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | tee /etc/apt/keyrings/ros.gpg > /dev/null

# Add ROS Noetic repo with signed-by option
RUN echo "deb [signed-by=/etc/apt/keyrings/ros.gpg] http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list

# Update and install ROS Noetic desktop full
RUN apt-get update && apt-get install -y ros-noetic-desktop-full

# Initialize rosdep
RUN rosdep init && rosdep update

# Setup environment variables
ENV LANG=C.UTF-8 \
    LC_ALL=C.UTF-8 \
    ROS_DISTRO=noetic
ENV ROS_PACKAGE_PATH=/opt/ros/noetic/share

# Create workspace and clone Interbotix repo
RUN mkdir -p /root/catkin_ws/src
WORKDIR /root/catkin_ws/src
RUN git clone https://github.com/Interbotix/interbotix_ros_manipulators.git

# Install ROS dependencies
WORKDIR /root/catkin_ws
RUN rosdep install --from-paths src --ignore-src -r -y

# Build workspace
RUN /bin/bash -c "source /opt/ros/noetic/setup.bash && catkin_make"

# Graphics setup environment variables
ENV DISPLAY=:0
ENV QT_X11_NO_MITSHM=1
ENV XDG_RUNTIME_DIR=/tmp

# Entrypoint
COPY ./ros_entrypoint.sh /
RUN chmod +x /ros_entrypoint.sh
ENTRYPOINT ["/ros_entrypoint.sh"]
CMD ["bash"]
