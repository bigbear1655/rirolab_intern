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

Loaded 'controller/joint_state'
Loaded 'controller/joint_state'

Loaded 'controller/trajectory'

Loaded 'controller/trajectory'

Started ['controller/joint_state'] successfully

Started ['controller/joint_state'] successfully

Started ['controller/trajectory'] successfully

Started ['controller/trajectory'] successfully

 
