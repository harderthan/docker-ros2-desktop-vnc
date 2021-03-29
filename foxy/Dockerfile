FROM dorowu/ubuntu-desktop-lxde-vnc:focal
LABEL maintainer="Tiryoh<tiryoh@gmail.com>"
# FROM rosplanning/navigation2:main.release
ENV DEBIAN_FRONTEND noninteractive
RUN echo "Set disable_coredump false" >> /etc/sudo.conf
RUN apt-get update -q && \
    apt-get upgrade -yq && \
    apt-get install -yq apt-utils wget curl git build-essential vim sudo lsb-release locales bash-completion tzdata gosu && \
    rm -rf /var/lib/apt/lists/*
RUN useradd --create-home --home-dir /home/ubuntu --shell /bin/bash --user-group --groups adm,sudo ubuntu && \
    echo ubuntu:ubuntu | chpasswd && \
    echo "ubuntu ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers
# Install ROS2 Desktop 
RUN  apt-get update && apt-get install -y curl gnupg2 lsb-release 
RUN  curl -Ls https://raw.githubusercontent.com/ros/rosdistro/master/ros.key | sudo apt-key add -
RUN  echo "deb http://packages.ros.org/ros2/ubuntu $(lsb_release -cs) main" > /etc/apt/sources.list.d/ros2-latest.list
RUN  apt-get update
RUN  apt-get install -y ros-foxy-desktop
RUN  apt-get install -y python3-argcomplete
RUN  apt-get install -y python3-colcon-common-extensions
RUN  apt-get install -y python3-argcomplete
RUN  apt-get install -y python3-colcon-common-extensions
RUN  apt-get install -y python3-rosdep python3-vcstool # https://index.ros.org/doc/ros2/Installation/Linux-Development-Setup/
RUN  rosdep init
RUN  rosdep update
RUN  apt-get install -y ros-foxy-gazebo-ros-pkgs
RUN  grep -F "source /opt/ros/foxy/setup.bash" ~/.bashrc || echo "source /opt/ros/foxy/setup.bash" >> ~/.bashrc
RUN  set -u
RUN ["/bin/bash", "-c", "source /opt/ros/foxy/setup.bash"]
RUN  echo "success installing ROS2 foxy"
RUN  echo "Run 'source /opt/ros/foxy/setup.bash'"
# Install navigation2
RUN  mkdir -p ~/Desktop/volume/navigation_ws/src && cd ~/Desktop/volume/navigation_ws/src && git clone https://github.com/ros-planning/navigation2.git  -b foxy-devel
RUN  cd ~/Desktop/volume/navigation_ws && rosdep install --from-path ./src -iy --rosdistro foxy 
# RUN  ["/bin/bash", "-c", "source /opt/ros/foxy/setup.bash && cd ~/Desktop/volume/navigation_ws && colcon build"] 
RUN rm -rf /var/lib/apt/lists/*
ENV  USER ubuntu
