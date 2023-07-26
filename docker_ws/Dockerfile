FROM osrf/ros:humble-desktop

MAINTAINER Hyunki Seong <hynkis@gmail.com>
LABEL Description="ROS-Humble-Desktop (Ubuntu 22.04)"

# Install packages without prompting the user to answer any questions
ENV DEBIAN_FRONTEND noninteractive 

# Install packages
RUN apt-get update
RUN apt-get update && apt-get install -y \
locales \
lsb-release \
mesa-utils \
git \
subversion \
nano \
terminator \
xterm \
wget \
curl \
htop \
tmuxp \
tmux \
libssl-dev \
build-essential \
dbus-x11 \
software-properties-common \
gdb valgrind && \
apt-get clean && rm -rf /var/lib/apt/lists/*

# Install cmake 3.22.1
RUN git clone https://gitlab.kitware.com/cmake/cmake.git && \
cd cmake && git checkout tags/v3.22.1 && ./bootstrap --parallel=8 && make -j8 && make install && \
cd .. && rm -rf cmake

# Install tmux 3.2
RUN apt-get update && apt-get install -y automake autoconf pkg-config libevent-dev libncurses5-dev bison && \
apt-get clean && rm -rf /var/lib/apt/lists/*
RUN git clone https://github.com/tmux/tmux.git && \
cd tmux && git checkout tags/3.2 && ls -la && sh autogen.sh && ./configure && make -j8 && make install

# Install new paramiko (solves ssh issues)
RUN apt-add-repository universe
RUN apt-get update && apt-get install -y python3-pip build-essential && apt-get clean && rm -rf /var/lib/apt/lists/*
RUN /usr/bin/yes | pip install --upgrade "pip < 21.0"
RUN /usr/bin/yes | pip install --upgrade virtualenv
RUN /usr/bin/yes | pip install --upgrade paramiko
RUN /usr/bin/yes | pip install --ignore-installed --upgrade numpy protobuf pyautogui

RUN apt-get update && apt-get install -y pcl-tools


# Locale
RUN locale-gen en_US.UTF-8  
ENV LANG en_US.UTF-8  
ENV LANGUAGE en_US:en  
ENV LC_ALL en_US.UTF-8

RUN apt-get update && apt-get install -y --allow-downgrades --allow-remove-essential --allow-change-held-packages \
libpcap-dev \
libopenblas-dev \
gstreamer1.0-tools libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev libgstreamer-plugins-good1.0-dev && apt-get clean && rm -rf /var/lib/apt/lists/*

# Install OhMyZSH
RUN apt-get update && apt-get install -y zsh && apt-get clean && rm -rf /var/lib/apt/lists/*
RUN wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | zsh || true
#RUN wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | zsh
RUN chsh -s /usr/bin/zsh root
RUN git clone https://github.com/sindresorhus/pure /root/.oh-my-zsh/custom/pure
RUN ln -s /root/.oh-my-zsh/custom/pure/pure.zsh-theme /root/.oh-my-zsh/custom/
RUN ln -s /root/.oh-my-zsh/custom/pure/async.zsh /root/.oh-my-zsh/custom/
RUN sed -i -e 's/robbyrussell/refined/g' /root/.zshrc
RUN sed -i '/plugins=(/c\plugins=(git git-flow adb pyenv tmux)' /root/.zshrc

# Terminator Config
RUN mkdir -p /root/.config/terminator/

# Install ROS libraries
RUN apt-get update && apt-get install -y ros-humble-ackermann-msgs ros-humble-tf2-msgs ros-humble-tf2 ros-humble-pcl-ros ros-humble-perception-pcl ros-humble-perception ros-humble-pcl-msgs ros-humble-pcl-conversions libgoogle-glog-dev

# Add user
# ref: https://stackoverflow.com/questions/28966198/dockerfile-home-is-not-working-with-add-copy-instructions
RUN useradd -m usrg
USER usrg


# Configure ROS
#RUN rosdep init && rosdep update 
RUN rosdep update 
RUN echo "source /opt/ros/humble/setup.bash" >> $HOME/.bashrc
RUN echo "export ROSLAUNCH_SSH_UNKNOWN=1" >> $HOME/.bashrc
RUN echo "source /opt/ros/humble/setup.zsh" >> $HOME/.zshrc
RUN echo "export ROSLAUNCH_SSH_UNKNOWN=1" >> $HOME/.zshrc

# configuration for ROS as USER
RUN echo "source $HOME/ros2_humble_docker/install/setup.bash" >> $HOME/.bashrc
RUN echo "export ROSLAUNCH_SSH_UNKNOWN=1" >> $HOME/.bashrc
RUN echo "alias cb='cd ~/ros2_humble_docker && colcon build --cmake-args -DCMAKE_BUILD_TYPE=Release'" >> $HOME/.bashrc

# ---
#CMD ["source $HOME/.bashrc"]
#CMD ["terminator"]
