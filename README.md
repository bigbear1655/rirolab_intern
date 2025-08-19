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

