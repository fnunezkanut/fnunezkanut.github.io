---
published: true
layout: post
title: 'Getting started with Ardone 2, Ubuntu, VirtualBox and ROS (Part 2)'
---
## Getting started with Ardone 2, Ubuntu, VirtualBox and ROS (Part 2)

Now that you have Ubuntu up and running in VirtualBox

<!--more-->

### Continuing on with installation...

Right so while still having the terminal open type the following (to get these installed):

	sudo apt-get install ros-kinetic-ardrone-autonomy ros-kinetic-joystick-drivers python-rosinstall

    sudo apt-get install libsdl1.2-dev libsdl-image1.2-dev libsdl-mixer1.2-dev libsdl-ttf2.0-dev libpulse-dev libxt-dev openssh-server


### Initialize rosdep
Might be a good idea to initialize rosdep and also add environmental variables, and update and reboot to be sure to be sure.

	sudo rosdep init
    rosdep update
    echo "source /opt/ros/kinetic/setup.bash" >> ~/.bashrc
	source ~/.bashrc
    sudo apt-get update
    reboot


### Configure Catkin and mikehamer/ardrone_tutorials package
Now we want to initialize catkin which we will need for the tutorial code (please note since i called my user **server** during setup my home directory is /home/server/

	mkdir /home/server/ros_workspace/
	mkdir /home/server/ros_workspace/src/
	cd /home/server/ros_workspace/src/
	catkin_init_workspace
	cd /home/server/ros_workspace/
    catkin_make
    source /home/server/ros_workspace/devel/setup.bash
	echo "source /home/server/ros_workspace/setup.bash" >> ~/.bashrc
	echo $ROS_PACKAGE_PATH

The result should be something like

    #$ /home/server/ros_workspace/src:/opt/ros/kinetic/share

![ros package path](https://fidel.ie/images/terminal2.jpg)

Now lets configure mikehamer/ardrone_tutorials package

    cd /home/server/ros_workspace/src/

    git clone https://github.com/mikehamer/ardrone_tutorials.git

    ls -la

    cd /home/server/ros_workspace/

    catkin_make

    rosmake -a

Well hopefully that installs the tutorials but we are not yet done :( since there is a dependancy on some python packages :O

	sudo apt-get install python-pyside


[Continued in Part 3](https://fidel.ie/2017/01/18/getting-started-with-ardrone2-part3.html)
