# rirolab_intern
Internship program for Geon Woo of RIRO_LAB
This is about how to set up teloperation for senseglove and shadowhand lite in Isaac-Sim



## Senseglove setup

### Publish topics from senseglove
First, open a new terminal.

```bash
source /opt/ros/noetic/setup.bash
cd senseglove_ws
roslaunch senseglove_launch senseglove_demo.launch
```

Then, you can check Sensecom works. If all topics are successfully published, you can check the below logs. If it does not work, you have to try again until it succeeds

Started ['controller/joint_state'] successfully

Started ['controller/joint_state'] successfully

Started ['controller/trajectory'] successfully

Started ['controller/trajectory'] successfully

As the topics are published in ros1, you need to bridge them to ros2.

### Bridge ros1 & ros2
Second, open a new terminal.

```bash
source /opt/ros/noetic/setup.bash
source /opt/ros/foxy/setup.bash
ros2 run ros1_bridge dynamic_bridge --bridge-all-topics
```

### Check fingertip topic (optional)
It is optional for checking topic /senseglove/rh/fingertip_positions

```bash
source /opt/ros/foxy/setup.bash
ros2 topic echo /senseglove/rh/fingertip_positions
```

/senseglove/rh/fingertip_positions topic refers to the xyz coordianate of thumb, index, middle, ring, pinky.

### Isaacsim teleoperation 
Third, 

