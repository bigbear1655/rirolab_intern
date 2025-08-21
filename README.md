# rirolab_intern
Internship program for Geon Woo of RIRO_LAB
This is about how to set up teloperation for senseglove and shadowhand lite in Isaac-Sim



## Senseglove setup

### Publish topics from senseglove
Open a new terminal.

```bash
source /opt/ros/noetic/setup.bash
cd senseglove_ws
source devel/setup.bash
roslaunch senseglove_launch senseglove_demo.launch
```

Then, you can check Sensecom works. If all topics are successfully published, you can check the below logs. If it does not work, you have to try again until it succeeds

Started ['controller/joint_state'] successfully

Started ['controller/joint_state'] successfully

Started ['controller/trajectory'] successfully

Started ['controller/trajectory'] successfully

As the topics are published in ros1, you need to bridge them to ros2.

### Check fingertip topic (optional)
It is optional for checking topic /senseglove/rh/fingertip_positions

```bash
source /opt/ros/foxy/setup.bash
ros2 topic echo /senseglove/rh/fingertip_positions
```

/senseglove/rh/fingertip_positions topic refers to the xyz coordianate of thumb, index, middle, ring, pinky.

### Bridge ros1 & ros2
You have to bridge ros1 message to ros2 message using dynamic bridge.
Open another terminal.

```bash
source /opt/ros/noetic/setup.bash
source /opt/ros/foxy/setup.bash
ros2 run ros1_bridge dynamic_bridge --bridge-all-topics
```

## Isaac sim teleoperation
You can teleoperate shadow-hand-lite in isaacsim.
Confirm the above senseglove setup.

```bash
conda activate isaac-sim-kgw
humble
cd isaacsim
./python.sh scripts/joint_sub.py
```

joint_sub.py
It subscribes / 


## shadow hand setup procedure
This should be implemented in shadow hand laptop. 
Follow the connecting procedures : <<https://shadow-robot-company-dexterous-hand.readthedocs-hosted.com/en/latest/user_guide/sh_connecting_cables.html>>
You must follow the procedure in order. 


### Gazebo
Export ROS_MASTER_URI in the docker. For not using *localhost*

```bash
export ROS_MASTER_URI=http://server:11311
```

roslaunch sr_robot_launch srhand.launch sim:=true hand_type:=hand_g

desktop_ip=192.168.0.61 
laptop_ip=192.168.0.171
server=10.9.11.1
nuc_control=10.9.11.2

