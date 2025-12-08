# FiveAges W2 Description

This package contains the description files for FiveAges W2 humanoid.

![FiveAges](.images/fiveages_w2.png)

## 1. Build

```bash
cd ~/ros2_ws
colcon build --packages-up-to fiveages_w2_description --symlink-install
```

## 2. Visualize the robot

### 2.1 Full Robot
* FiveAges W2 (Basic visualization)
  ```bash
  source ~/ros2_ws/install/setup.bash
  ros2 launch robot_common_launch humanoid.launch.py robot:=fiveages_w2 collider:=simple
  ```

<details>
<summary><strong>More Configs with End Effector</strong></summary>

* With ChangingTek AG2F90 Gripper
  ```bash
  source ~/ros2_ws/install/setup.bash
  ros2 launch robot_common_launch humanoid.launch.py robot:=fiveages_w2 collider:=simple type:=ag2f90
  ```
* With Jodell RG75 Gripper
  ```bash
  source ~/ros2_ws/install/setup.bash
  ros2 launch robot_common_launch humanoid.launch.py robot:=fiveages_w2 collider:=simple type:=rg75
  ```
  
</details>

### 2.2 Component
* Linkhou S2 Base
  ```bash
  source ~/ros2_ws/install/setup.bash
  ros2 launch robot_common_launch component.launch.py robot:=fiveages_w2
  ```
* Body
  ```bash
  source ~/ros2_ws/install/setup.bash
  ros2 launch robot_common_launch component.launch.py robot:=fiveages_w2 type:=body
  ```

## 3. ROS2 Control Demos

### 3.1 Mock Component
* Full Body Control
  ```bash
  source ~/ros2_ws/install/setup.bash
  ros2 launch ocs2_arm_controller full_body.launch.py robot:=fiveages_w2
  ```
* Split Body Control
  ```bash
  source ~/ros2_ws/install/setup.bash
  ros2 launch ocs2_arm_controller split_body.launch.py robot:=fiveages_w2
  ```
* Only Body
  ```bash
  source ~/ros2_ws/install/setup.bash
  ros2 launch basic_joint_controller demo.launch.py robot:=fiveages_w2
  ```
<details>
<summary><strong>More Full Body Configs</strong></summary>

* With ChangingTek AG2F90 Gripper
  ```bash
  source ~/ros2_ws/install/setup.bash
  ros2 launch ocs2_arm_controller full_body.launch.py type:=ag2f90 robot:=fiveages_w2
  ```
</details>

### 3.2 Isaac Sim
* Empty Hand (Full Body Control)
  ```bash
  source ~/ros2_ws/install/setup.bash
  ros2 launch ocs2_arm_controller full_body.launch.py robot:=fiveages_w2 hardware:=isaac
  ```

### 3.3 Joint Control Commands
* Set Head Angles
  ```bash
  ros2 topic pub /head_joint_controller/target_joint_position std_msgs/msg/Float64MultiArray "{data: [0.0, 0.5]}"  # 设置目标关节角度（弧度）
  ```
  ```bash
  ros2 topic pub /head_joint_controller/target_joint_position std_msgs/msg/Float64MultiArray "{data: [0.0, 0.0]}"  # 设置目标关节角度（弧度）
  ```
* Set Body Angles (Only available on split control)
  ```bash
  ros2 topic pub /body_joint_controller/target_joint_position std_msgs/msg/Float64MultiArray "{data: [1.0145214890243484, -2.248923594021317, -1.2333776023323322, 0.0]}"  # 设置目标关节角度（弧度）
  ```
  ```bash
  ros2 topic pub /body_joint_controller/target_joint_position std_msgs/msg/Float64MultiArray "{data: [0.6810335903307411, -1.5096724218776123, -0.8279494910068607, 0.0]}"  # 设置目标关节角度（弧度）
  ```
* Set Arm Angles
  ```bash
  ros2 topic pub /ocs2_arm_controller/target_joint_position std_msgs/msg/Float64MultiArray "{data: [0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0,0.0,0.0,0.0]}"  # 设置目标关节角度（弧度）
  ```

## 4. Real Robot Deploy
* Only Body
  ```bash
  source ~/ros2_ws/install/setup.bash
  ros2 launch basic_joint_controller demo.launch.py robot:=fiveages_w2 hardware:=real enable_head:=false
  ```