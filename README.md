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
  source ~/fa_w2_ws/install/setup.bash
  ros2 launch robot_common_launch humanoid.launch.py robot:=fiveages_w2 collider:=simple
  ```

<details>
<summary><strong>More Configs with End Effector</strong></summary>

* With ChangingTek AG2F90 Gripper
  ```bash
  source ~/ros2_ws/install/setup.bash
  ros2 launch robot_common_launch humanoid.launch.py robot:=fiveages_w2 collider:=simple type:=ag2f90
  ```
* With ChangingTek AG2F120S Gripper
  ```bash
  source ~/ros2_ws/install/setup.bash
  ros2 launch robot_common_launch humanoid.launch.py robot:=fiveages_w2 collider:=simple type:=ag2f120s
  ```
* With ChangingTek AG2F120S-D Gripper
  ```bash
  source ~/ros2_ws/install/setup.bash
  ros2 launch robot_common_launch humanoid.launch.py robot:=fiveages_w2 collider:=simple type:=ag2f120s_d
  ```
* With Jodell RG75 Gripper
  ```bash
  source ~/ros2_ws/install/setup.bash
  ros2 launch robot_common_launch humanoid.launch.py robot:=fiveages_w2 collider:=simple type:=rg75
  ```
* With LinkerHand O6
  ```bash
  source ~/ros2_ws/install/setup.bash
  ros2 launch robot_common_launch humanoid.launch.py robot:=fiveages_w2 collider:=simple type:=linkerhand_o6
  ```
* With LinkerHand O7
  ```bash
  source ~/ros2_ws/install/setup.bash
  ros2 launch robot_common_launch humanoid.launch.py robot:=fiveages_w2 collider:=simple type:=linkerhand_o7
  ```

</details>

### 2.2 Component

* Dual arms (default)
  ```bash
  source ~/ros2_ws/install/setup.bash
  ros2 launch robot_common_launch component.launch.py robot:=fiveages_w2
  ```
* LP02 camera
  ```bash
  source ~/ros2_ws/install/setup.bash
  ros2 launch robot_common_launch component.launch.py robot:=fiveages_w2 type:=lp02_336_camera
  ```

Chassis (Linkhou S2), body, and head component visualization live in `fiveages_w2_common_description`. See [fiveages_w2_common_description/README.md](../fiveages_w2_common_description/README.md).

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
* With Jodell RG75 Gripper
  ```bash
  source ~/ros2_ws/install/setup.bash
  ros2 launch ocs2_arm_controller full_body.launch.py type:=rg75 robot:=fiveages_w2
  ```
* With Jodell RG75 Gripper and Heavy Pad
  ```bash
  source ~/ros2_ws/install/setup.bash
  ros2 launch ocs2_arm_controller full_body.launch.py type:=rg75_heavy robot:=fiveages_w2
  ```
* With LinkerHand O6
  ```bash
  source ~/ros2_ws/install/setup.bash
  ros2 launch ocs2_arm_controller full_body.launch.py type:=linkerhand_o6 robot:=fiveages_w2
  ```
* With LinkerHand L6
  ```bash
  source ~/ros2_ws/install/setup.bash
  ros2 launch ocs2_arm_controller full_body.launch.py type:=linkerhand_l6 robot:=fiveages_w2
  ```

* With LinkerHand O7
  ```bash
  source ~/ros2_ws/install/setup.bash
  ros2 launch ocs2_arm_controller full_body.launch.py type:=linkerhand_o7 robot:=fiveages_w2
  ```

</details>

<details>
<summary><strong>More Split Body Configs</strong></summary>

* With ChangingTek AG2F90 Gripper
  ```bash
  source ~/ros2_ws/install/setup.bash
  ros2 launch ocs2_arm_controller split_body.launch.py type:=ag2f90 robot:=fiveages_w2
  ```
  ```bash
  source ~/ros2_ws/install/setup.bash
  ros2 launch ocs2_arm_controller split_body.launch.py type:=ag2f90_d robot:=fiveages_w2
  ```
* With Jodell RG75 Gripper
  ```bash
  source ~/ros2_ws/install/setup.bash
  ros2 launch ocs2_arm_controller split_body.launch.py type:=rg75 robot:=fiveages_w2
  ```

* With Jodell RG75 Gripper and Heavy Pad
  ```bash
  source ~/ros2_ws/install/setup.bash
  ros2 launch ocs2_arm_controller split_body.launch.py type:=rg75_heavy robot:=fiveages_w2
  ```
* With LinkerHand O6
  ```bash
  source ~/ros2_ws/install/setup.bash
  ros2 launch ocs2_arm_controller split_body.launch.py type:=linkerhand_o6 robot:=fiveages_w2
  ```
* With LinkerHand L6
  ```bash
  source ~/ros2_ws/install/setup.bash
  ros2 launch ocs2_arm_controller split_body.launch.py type:=linkerhand_l6 robot:=fiveages_w2
  ```

</details>

### 3.2 Isaac Sim

* Empty Hand (Full Body Control)
  ```bash
  source ~/ros2_ws/install/setup.bash
  ros2 launch ocs2_arm_controller full_body.launch.py robot:=fiveages_w2 hardware:=isaac
  ```
* With Jodell RG75 Gripper
  ```bash
  source ~/ros2_ws/install/setup.bash
  ros2 launch ocs2_arm_controller split_body.launch.py type:=rg75 robot:=fiveages_w2 hardware:=isaac
  ```
* With ChangingTek AG2F90 Gripper
  ```bash
  source ~/ros2_ws/install/setup.bash
  ros2 launch ocs2_arm_controller split_body.launch.py type:=ag2f90 robot:=fiveages_w2 hardware:=isaac
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

## 5. Navigation (Isaac Sim / 点云雷达)

底盘障碍感知使用前后雷达点云：`/fl_lidar/point_cloud`、`/rr_lidar/point_cloud`（参数见 `config/nav2/nav2_params_isaac_gt.yaml`）。

```bash
source ~/ros2_ws/install/setup.bash
ros2 launch robot_common_launch navigation_isaac_gt.launch.py robot:=fiveages_w2
```

```bash
source ~/ros2_ws/install/setup.bash
ros2 launch robot_common_launch navigation_isaac_gt.launch.py robot:=fiveages_w2 map:=warehouse_multiple_shelfs nav2_profile:=map_only
```

### 5.1 导航地图与 Nav2 配置

本包通过 **`config/navigation.yaml`** 声明导航资源所在包（默认指向 `fiveages_w2_common_description`）：

```yaml
map_config_package: fiveages_w2_common_description
nav_config_package: fiveages_w2_common_description
```

实际地图与 Nav2 文件位于 `fa-w2-components` 子模块：

- 地图：`fiveages_w2_common_description/config/maps/`（如 `siemens`、`siemens_task2`）
- Nav2：`fiveages_w2_common_description/config/nav2/`

`robot:=fiveages_w2` 时 launch 读取上述 `navigation.yaml`，再 fallback 到 `robot_common_launch/config/`。无需在 launch 参数里写死包名。

```bash
ros2 launch robot_common_launch navigation_isaac_gt.launch.py \
  robot:=fiveages_w2 map:=siemens_task2
```

工作区 `./quick_start.sh` → 启动 → 导航 会**自动扫描**可用地图列表。

详见 [`fa-w2-components` README](../fa-w2-components/README.md) 导航章节。