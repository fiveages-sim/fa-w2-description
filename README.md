# FiveAges W2 Description

轮式人形 W2 的 URDF / ros2_control 描述包。模型在 **`xacro/`**；底盘 / 躯干 / 头部在 [`fiveages_w2_common_description`](../fiveages_w2_common_description/README.md)；双臂与末端复用 [`m6_ccs_description`](../../manipulator/Tianji/m6_ccs_description/README.md)（`M6_CCS` 臂、`eefs.xacro`、共享 ros2_control 宏）。

```bash
cd ~/ros2_ws && colcon build --packages-up-to fiveages_w2_description --symlink-install
source install/setup.bash
```

![FiveAges W2](.images/fiveages_w2.png)

## 1. 可视化与 Launch

| 场景 | 入口 | 示例 |
|------|------|------|
| 全身 RViz | `humanoid.launch.py` | `… robot:=fiveages_w2 collider:=simple` |
| 带末端 | 同上 + `type:=<key>` | `… type:=erg32`（键名见 [m6_ccs §2](../../manipulator/Tianji/m6_ccs_description/README.md#2-末端执行器)） |
| 左右不同末端 | `left_type` / `right_type` | `left_type:=rg75 right_type:=linkerhand_o7` |
| 部件调试 | `component.launch.py` | `… type:=lp02_336_camera` |
| 全身 WBC（mock） | `ocs2_arm_controller full_body.launch.py` | 默认 `hardware:=mock_components` |
| 分体控制（mock） | `split_body.launch.py` | 腰 / 头 / 臂独立控制器 |
| 仅腰体 | `basic_joint_controller demo.launch.py` | `robot:=fiveages_w2` |
| Isaac | 上述 launch + `hardware:=isaac` | Topic 桥接仿真 |
| 导航（仿真） | `navigation_isaac_gt.launch.py` | 见 [§6](#6-导航) |

```bash
ros2 launch robot_common_launch humanoid.launch.py robot:=fiveages_w2 collider:=simple type:=erg32
ros2 launch robot_common_launch humanoid.launch.py robot:=fiveages_w2 collider:=simple type:=rg75
ros2 launch ocs2_arm_controller full_body.launch.py robot:=fiveages_w2 type:=erg32
ros2 launch ocs2_arm_controller split_body.launch.py robot:=fiveages_w2 type:=erg32
ros2 launch ocs2_arm_controller full_body.launch.py robot:=fiveages_w2 type:=linkerhand_o7
```

## 2. 末端执行器

`type` / `left_type` / `right_type` 与 m6_ccs **完全相同**（解析：`m6_ccs_description/xacro/eef_type_resolver.xacro`）。

真机驱动方式、支持矩阵、CAN 限制等详见 **[m6_ccs README §2](../../manipulator/Tianji/m6_ccs_description/README.md#2-末端执行器)**，此处不重复。

W2 特有：`hardware:=real` 时臂走 Marvin + `juxie_body`（腰头 CAN）；外接 `usb` / `usb_can` 末端逻辑与 m6_ccs 共用 `external_ee_systems`。

## 3. 平台与模型

| 层级 | 包 / xacro | 说明 |
|------|------------|------|
| 底盘 | `fiveages_w2_common` `linkhou_s2` | launch：`chassis:=linkhou_s2`（默认）/ `linkhou_s2_v2` / `linkhou_s2_sinopec` |
| 躯干 / 头 | `fiveages_w2_common` `body.xacro` / `head.xacro` | OEM 外观：`variant:=sinopec` 等 |
| 双臂 | `m6_ccs` `M6_CCS` | `skin:=fiveages_w2`；碰撞与 m6 共用 `link*.glb` |
| 末端 | `m6_ccs` `eefs.xacro` | 与 m6_ccs 一致 |

规划 URDF 默认冻结底盘轮关节与末端主动关节（`chassis_joints_movable:=false`、`eef_fixed_joints` 由 launch 设置）。

## 4. ros2_control 架构

真机拆为 **两个 system**；仿真合并为一个：

| 模式 | System | 硬件插件 |
|------|--------|----------|
| 真机 | `M6_CCS_System` | `marvin_ros2_control`（双臂 + 夹爪/手） |
| 真机 | `juxie_body` | `juxie_ros2_control`（`body_joint1~4`、`head_joint1~2`） |
| 真机 | 外接末端 *\_system | `modbus_ros2_control` / `can_ros2_control`（按 drive type） |
| 仿真 | `fiveages_w2_system` | mock / gz / isaac（[`robot_common_launch` simulation_hardware](../../common/robot_common_launch/xacro/ros2_control/simulation_hardware.xacro)） |

手臂侧宏均来自 [m6_ccs §5](../../manipulator/Tianji/m6_ccs_description/README.md#5-ros2_control-共享宏)：`dual_arm_ee_block`、`marvin_hardware_block`、`external_ee_systems` 等。本包 `xacro/ros2_control/robot.xacro` 仅为薄封装。

控制器配置：`config/ros2_control/common.yaml`（`ocs2_wbc_controller` 全身、`ocs2_arm_controller` 双臂、`body_joint_controller` / `head_joint_controller` 等）。

## 5. 真机部署

```bash
colcon build --packages-up-to marvin_ros2_control modbus_ros2_control can_ros2_control juxie_ros2_control --symlink-install
source install/setup.bash

# 仅腰体（无臂）
ros2 launch basic_joint_controller demo.launch.py robot:=fiveages_w2 hardware:=real enable_head:=false

# 全身真机 + 末端（示例：夹爪 / 外接手）
ros2 launch ocs2_arm_controller full_body.launch.py robot:=fiveages_w2 hardware:=real type:=ag2f90_c
ros2 launch ocs2_arm_controller full_body.launch.py robot:=fiveages_w2 hardware:=real type:=erg32
ros2 launch ocs2_arm_controller split_body.launch.py robot:=fiveages_w2 hardware:=real_usb type:=linkerhand_o7
ros2 launch ocs2_arm_controller split_body.launch.py robot:=fiveages_w2 hardware:=real_usb type:=inspire_f2 \
  left_ee_drive_type:=usb_can right_ee_drive_type:=usb_can
```

`robot.local.yaml` 片段（`hardware:` 在 `hardware:=real` / `real_usb` 时注入 xacro）：

```yaml
defaults:
  end_effectors:
    type: ag2f90_c
  ft:
    left: kwr75_485
    right: none
  tcp_offset:
    xyz: "0 0 0"
    rpy: "0 0 0"              # radians; e.g. "0 0 ${PI/2}"
hardware:
  device_ip: 192.168.1.190
  arm_ctrl_mode: position
  max_joint_speed: 50
  max_joint_acceleration: 30
  body_can_interface: can0
  usb_left_port: /dev/ttyUSB0
  usb_right_port: /dev/ttyUSB2
  left_ee_can_interface: can0
  right_ee_can_interface: can1
  left_read_tactile: "false"
  right_read_tactile: "false"
  left_ee_drive_type: marvin
  right_ee_drive_type: marvin
```

Launch 可覆盖：`ft:=…`、`tcp_offset_rpy:=…`、`hardware_device_ip:=…`、`hardware_max_joint_speed:=…`、`hardware_body_can_interface:=…`、`hardware_usb_left_port:=…`、`hardware_left_ee_can_interface:=…`、`hardware_left_read_tactile:=true`、`hardware_left_ee_drive_type:=usb_can` 等（规则同 m6_ccs）。

### 分体控制常用话题（`split_body`）

```bash
# 头
ros2 topic pub /head_joint_controller/target_joint_position std_msgs/msg/Float64MultiArray "{data: [0.0, 0.5]}"
# 腰（仅 split_body）
ros2 topic pub /body_joint_controller/target_joint_position std_msgs/msg/Float64MultiArray \
  "{data: [1.014, -2.249, -1.233, 0.0]}"
# 双臂（14 维）
ros2 topic pub /ocs2_arm_controller/target_joint_position std_msgs/msg/Float64MultiArray \
  "{data: [0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0]}"
```

## 6. 导航

点云话题：`/fl_lidar/point_cloud`、`/rr_lidar/point_cloud`（见 `config/nav2/nav2_params_isaac_gt.yaml`）。

本包 `config/navigation.yaml` 将地图 / Nav2 指向 `fiveages_w2_common_description`：

```yaml
map_config_package: fiveages_w2_common_description
nav_config_package: fiveages_w2_common_description
```

```bash
ros2 launch robot_common_launch navigation_isaac_gt.launch.py robot:=fiveages_w2
ros2 launch robot_common_launch navigation_isaac_gt.launch.py robot:=fiveages_w2 map:=siemens_task2
ros2 launch robot_common_launch navigation_isaac_gt.launch.py \
  robot:=fiveages_w2 map:=warehouse_multiple_shelfs nav2_profile:=map_only
```

地图与 Nav2 文件：`fiveages_w2_common_description/config/maps/`、`config/nav2/`。工作区 `quick_start.sh` 可扫描可用地图。详见 [fa-w2-components README](../fa-w2-components/README.md)。
