# *raspicam_node* ROS package

This repository represents *raspicam_node* ROS package, the drive for the Raspberry Pi Camera Module. In the experiment, [Camera Module v2](https://www.raspberrypi.org/products/camera-module-v2/) is used. This repository is obtained and modified from [this](https://github.com/UbiquityRobotics/raspicam_node) GitHub repository. However, the instructions are available only for ROS Kinetic at the moment of writing. This is because mmal libraries are not available for Ubuntu 18.04 for 64-bit architecture. Below are the instruction on how to build raspberry pi *userland* and use *raspicam_node* driver with ROS Melodic on Ubuntu 18.04.

## Setup guide

1. **Build userland**
First, clone the *userland* repository to a desired location with following command:
```
git clone https://github.com/raspberrypi/userland.git
```
Next, go to the userland location with:
```
cd userland
```
and execute:
```
git checkout 2fe4ca3
```
Finally, build userland with:
```
./buildme --aarch64
```
2. **Configuration**
Open the *rpi.conf* file in editor with:
```
sudo vim /etc/ld.so.conf.d/rpi.conf
```
and append `/opt/vc/lib` in the file. After saving the changes, run:
```
sudo ldconfig
``` 
Next, open a file with permission rules in editor with: 
```
sudo vim  /etc/udev/rules.d/10-vchiq-permissions.rules
``` 
and add the following line `SUBSYSTEM=="vchiq",GROUP="video",MODE="0660"`. After saving changes, run: 
```
sudo usermod -aG video ubuntu
``` 
Next, open a config file in editor with: 
```
sudo vim /boot/firmware/config.txt
```
and append `gpu_mem=128` and `start_x=1` to it. After saving the changes, reboot the robotic system with:
```
sudo reboot
``` 
Upon rebooting, open a file in editor with:
```
sudo vim /etc/ros/rosdep/sources.list.d/30-ubiquity.list
```
and place `yaml https://raw.githubusercontent.com/UbiquityRobotics/rosdep/master/raspberry-pi.yaml` in the file. Finally, run:
```
rosdep update
```
3. **Build raspicam_node**: 
Go to the *src* directory of your catkin workspace with: 
```
cd <catkin_ws_dir>/src
```
and clone this repository with:
```
git clone git@github.com:minana96/raspicam_node.git
```
Next, return the to catking workspace with:
```
cd ..
``` 
and run: 
```
rosdep install --from-paths src --ignore-src --rosdistro=melodic
``` 
Finally, execute: 
```
catkin_make
```

---

## ROS2

Install Compressed Image Transport
```
$ sudo apt install ros-foxy-compressed-image-transport
```

Follow this: https://github.com/christianrauch/raspicam2_node
