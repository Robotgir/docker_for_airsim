# docker_for_airsim
This repo aims to create, maintain Dockerfiles and ready to use docker containers for now archieved AirSim simulator for various configurations and maintain them as this simulator has potential use in the field of UAV and Robotics development.

Grab the docker image from [docker hub](https://hub.docker.com/u/giri6937).  This repo provides the images in:
* [giri6937/ue_airsim_inside_docker](https://hub.docker.com/r/giri6937/ue_airsim_inside_docker/tags)
check out the ta  


  
## Description

- This docker container has UE(without editor), ros2 foxy installed and is compatible with Airsim.
- Please follow below instructions to succesfully launch the environment.
- Tested on Ubuntu: 20.04 host, 
Nvidia driver & kernel combination: 555.42.02 & 5.11.5-051105-generic and 470.256.02 & 5.15.0-107-generic

### Before launching the container:

- install vulkan library
    
    `sudo apt install libvulkan1`
    
- we use the local AirSim repository and mount it into the container. This way you can still continue using git with AirSim in host system. To do this make a note of the path to the AirSim repo in your host system.
- We can choose from precompiled binaries of the UE environments provided at https://github.com/microsoft/AirSim/releases, under linux section download the environment of choice and make a not of the path to this folder.
- make sure docker is installed.
- make sure nvidia-driver  and NVIDIA Container Toolkit are installed. Also important is to configure usgae of nvidia driver with docker this is described in https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html : under section **Configuration/** **Configuring Docker**

### To launch the container:

- use below command to launch the container, make sure you update the path to AirSim repo and UEenvironment binary.

`sudo docker run --rm -it --privileged -e DISPLAY=$DISPLAY -e XAUTHORITY=$XAUTHORITY -e XDG_RUNTIME_DIR=/run/user/$UID -v /usr/share/vulkan/icd.d:/usr/share/vulkan/icd.d -e XR_LOADER_DEBUG=all  -v /tmp/.X11-unix:/tmp/.X11-unix -v $XAUTHORITY:$XAUTHORITY -v /home/PATH_TO_AirSim/AirSim:/home/airsim_user/workspace/AirSim -v /home/PATH_TO_DOWNLOADED_ENVIRONMENT/AbandonedPark:/home/airsim_user/Downloads --gpus all --entrypoint /bin/bash giri6937/ue_airsim_inside_docker:foxy`
- Inside the container enter into the directery where the script file to launch the UE environment is for example:
`cd Downloads/LinuxNoEditor/`
launch the UE environment with below command
    
    `./AbandonedPark.sh -windowed -ResX=1080 -ResY=720`

### Testing Airsim:

- build the ros2 workspace within AirSim repo using following command
    
    `cd /AirSim/ros2`
    
    `colcon build --cmake-args -DCMAKE_C_COMPILER=gcc-8 --cmake-args -DCMAKE_CXX_COMPILER=g++-8 --cmake-args -DCMAKE_BUILD_TYPE=Debug`
    
- in tab 1
    
    `source install/setup.bash`
    
    `ros2 launch airsim_ros_pkgs airsim_node.launch.py`
    
- in tab 2
    
    `ros2 topic list`
