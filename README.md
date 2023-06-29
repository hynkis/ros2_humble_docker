# ROS2 Humble Docker Environment
Dockerfile for ROS2 Humble. The followings are considered:
  - Run docker container with Terminator
  - rviz2 GUI visualization in the container
  - user permission
  - ROS communication between the container and host PC
  - shared directory option

## Installation
```
cd ~
git clone https://github.com/hynkis/ros2_humble_docker.git
cd ros2_humble_docker/docker_ws
docker build -t osrf/ros-humble-user .
```

## Docker run command
```
docker run --rm -it --privileged \
--net=host \
--ipc=host \
--device=/dev/dri:/dev/dri \
-v /tmp/.X11-unix:/tmp/.X11-unix \
-v /home/usrg/.ros:/home/usrg/.ros \
-v $HOME/.Xauthority:/home/$(id -un)/.Xauthority \
-e DISPLAY=$DISPLAY \
-e XAUTHORITY=/home/$(id -un)/.Xauthority \
-u "$(id -u $USER):$(id -g $USER)" \
-v /etc/passwd:/etc/passwd:ro \
-v /etc/group:/etc/group:ro \
-v $HOME/ros2_humble_docker:$HOME/ros2_humble_docker \
-e ROS_IP=127.0.0.1 osrf/ros-humble-user:latest
```
