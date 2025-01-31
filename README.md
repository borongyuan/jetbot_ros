# jetbot_ros
ROS nodes for NVIDIA JetBot with Jetson Nano


### Install ROS Melodic

```bash
$ sudo apt-add-repository universe
$ sudo apt-add-repository multiverse
$ sudo apt-add-repository restricted

# Setup sources.lst
$ sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'

# Setup keys
$ sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net:80 --recv-key 0xB01FA116


$ sudo apt-get update
$ sudo apt-get install ros-melodic-ros-base


$ echo "source /opt/ros/melodic/setup.bash" >> ~/.bashrc
```

Close and restart the terminal.


### Install Adafruit-MotorHAT python library

```bash
$ sudo apt-get install python-pip
$ pip install Adafruit-MotorHAT
```


### Create catkin workspace

> Note:  my catkin_ws is created as a subdirectory under ~/workspace out of personal preference

```bash
$ mkdir -p ~/workspace/catkin_ws/src
$ cd ~/workspace/catkin_ws
$ catkin_make

# add catkin_ws path to bashrc
$ echo "source ~/workspace/catkin_ws/devel/setup.bash" >> ~/.bashrc
```

Close and open a new terminal window.

```bash
$ echo $ROS_PACKAGE_PATH 
/home/nvidia/catkin_ws/src:/opt/ros/melodic/share
```

### Clone and build the jetbot_ros package

```bash
$ cd ~/workspace/catkin_ws/src
$ git clone https://github.com/dusty-nv/jetbot_ros

# build the package
$ cd ../    # cd ~/workspace/catkin_ws
$ catkin_make

# confirm that jetbot_ros package can be found
$ rospack find jetbot_ros
/home/nvidia/workspace/catkin_ws/src/jetbot_ros
```


### Testing the Motors

Open a new terminal, and start `roscore`
```bash
$ roscore
```

Open a new terminal, and start the `jetbot_motors` node:
```bash
$ rosrun jetbot_ros jetbot_motors.py
```

The `jetbot_motors` node will listen on the following topics:
	* `/jetbot_motors/cmd_dir`     relative direction (degree [-180,180], speed [-1,1])
	* `/jetbot_motors/cmd_raw`     raw L/R motor commands  (speed [-1,1], speed [-1,1])
	* `/jetbot_motors/cmd_str`     simple string commands (left/right/forward/backward/stop)

> Note that of 2/22/19, only `cmd_str` method is implemented.  Other methods coming soon.

Open a new terminal, and run some test commands:
(it is recommended to initially test with JetBot up on blocks, wheels not touching the ground)

```bash
$ rostopic pub /jetbot_motors/cmd_str std_msgs/String --once "forward"
$ rostopic pub /jetbot_motors/cmd_str std_msgs/String --once "backward"
$ rostopic pub /jetbot_motors/cmd_str std_msgs/String --once "left"
$ rostopic pub /jetbot_motors/cmd_str std_msgs/String --once "right"
$ rostopic pub /jetbot_motors/cmd_str std_msgs/String --once "stop"
```


