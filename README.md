# RIROLAB_intern
Internship program for Geon Woo in RIRO_LAB.

This is about how to set up teloperation for senseglove and shadowhand lite in Isaac-Sim

## Senseglove setup

#### Publish topics from senseglove
Open a new terminal.

```bash
source /opt/ros/noetic/setup.bash
cd senseglove_ws
source devel/setup.bash
roslaunch senseglove_launch senseglove_demo.launch
```

Then, you can check Sensecom works. If all topics are successfully published, you can check the below logs. If it does not work, you have to try again until it succeeds
```bash
Started ['controller/joint_state'] successfully
Started ['controller/joint_state'] successfully
Started ['controller/trajectory'] successfully
Started ['controller/trajectory'] successfully
```
As the topics are published in ros1, you need to bridge them to ros2.

#### Check fingertip topic (optional)
It is optional for checking topic /senseglove/rh/fingertip_positions

```bash
source /opt/ros/foxy/setup.bash
ros2 topic echo /senseglove/rh/fingertip_positions
```

/senseglove/rh/fingertip_positions topic refers to the xyz coordianate of thumb, index, middle, ring, pinky.

#### Bridge ros1 & ros2
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

It subscribes /senseglove/rh/fingertip_positions, and publishes shadow hand joint position using GeoRT, retargeting method through Isaacsim.

You can choose the trained model through ```checkpoint_tag``` in line 104. 

The trained model is in /GeoRT/checkpoint.

The training procedure is described below.

## Train retargeting model
We used retargeting method of GeoRT: <<https://github.com/facebookresearch/GeoRT>>

Project website: <<https://zhaohengyin.github.io/geort/>>

It retargets five fingertip coordinates to the joint positions in robot hand using FK neural network.

#### Motion capture
You need to record fingertip positions with Senseglove.

Be confirmed if senseglove publishes the topic '/senseglove/rh/fingertip_positions' and ros bridge is working.

Open another terminal.

```bash
conda activate isaac-sim-kgw
cd GeoRT
python geort/senseglove_mocap.py
```
```senseglove_mocap.py``` subscribes ```/senseglove/rh/fingergip_positions``` and captures coordinates of fingertips to npy file. 

During motion capture, repeat folding and stretching hands. Perform several pinch movements with your fingers.  

And you need to stretch each finger as much as possible to accumulate data.
It may take 5 minutes. 

Give a name of the dataset with  ```data_output_name``` in ```main()```.

It is saved in ```data/{your dataset name}.npy```

#### Training
At the same terminal,

```bash
python geort/trainer.py -human_data {your dataset name} -ckpt_tag {your model name}
```

Your trained retargeting model is saved in '/GeoRT/checkpoint'.

Training takes almost 3 minutes. You can check your model in Isaac-sim with ```joint_sub.py```, described above.

## shadow hand setup procedure
You can teleoperate real shadow hand with trained model. 

This should be implemented in shadow hand laptop. 

Follow the connecting procedures : <<https://shadow-robot-company-dexterous-hand.readthedocs-hosted.com/en/latest/user_guide/sh_connecting_cables.html>>

You must follow the procedure in order. If you don't, roscore problem will occur. Check the orange light of ethernet cables before you go next step.

**important** 'LAPTOP NUC' is connected with your laptop and 'NUC TO LAPTOP' with NUC. If not, roscore problem occurs.

#### Launch 
If they are correctly connected, click 'Shadow Launcher Dext..' and 'Launch Shadow Right Hand.desktop'.

Then, Server container, roscore, ... start to be executed.

In desktop, you need to publish topic about joint positions.

At the same terminal that **Publish topics from senseglove** above, change ros master uri to shadow hand nuc. 

```bash
export ROS_MASTER_URI=http://server:11311
source devel/setup.bash
roslaunch senseglove_launch senseglove_demo.launch
```
Open a new terminal.

```bash
conda activate shadow_teleop
source /opt/ros/noetic/setup.bash
cd ~/shadow_ws/src/fingertip_to_qpos/scripts
python fingertip_to_qpos.py -checkpoint_tag {your model name}
```

fingertip_to_qpos.py

This script subscribes fingertip positions of senseglove and publishes joint position. 

In container, 
```bash
roscd sr_example/scripts/sr_example/kgw_scripts
python teleoperate.py
```
#### Shut down
Click 'Shadow Close Everything.desktop' before you turn off the shadow hand.

Deconnect everything along the connecting procedure in reverse order. 


----------------
## Memo
roslaunch sr_robot_launch srhand.launch sim:=true hand_type:=hand_g

desktop_ip=192.168.0.61 
laptop_ip=192.168.0.171
server=10.9.11.1
nuc_control=10.9.11.2

