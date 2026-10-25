---
layout: default
title: Developer 接口
nav_order: 4.2
parent: 参考指南
has_toc: true
---

# 参考指南 Developer 接口

* TOC
{:toc}

> ℹ️ **说明**：
>
> 使用 Fourier-GRX-N1 SDK Developer API 前，请将 `fourier-grx` 配置为 **开发者模式**。
> 关于运行模式的配置，请参见 [运行模式](/fourier-grx-N1/docs/reference/run_type)。

Fourier-GRX Developer 接口是针对开发者底层开发提供的二次开发接口，直接调用底层硬件接口，适用于底层开发。

需要在 python 环境中安装 fourier_core 和 fourier_grx 库后，直接使用库函数进行开发调用。

---

## 4.0.0 以上版本

### 接口调用方式

```python
import fourier_grx.sdk.developer as fourier_grx

control_system = fourier_grx.ControlSystem()

# 初始化开发者模式（servo_on=False 时执行器保持失能；control_frequency 单位为 Hz，上限 500）
control_system.developer_mode(servo_on=False, control_frequency=100)

# 版本信息
info = control_system.get_info()  # {"fourier_core_version": ..., "fourier_grx_version": ...}

# 读取状态 / 下发控制（字典格式见下文）
state_dict = control_system.robot_control_loop_get_state()
control_system.robot_control_loop_set_control(control_dict=control_dict)

# 发送任务指令
control_system.robot_control_set_task_command(task_command=fourier_grx.TaskCommand.TASK_IDLE)
```

`fourier_grx.TaskCommand` 提供常用任务值常量（`TASK_IDLE`、`TASK_SERVO_ON`、`TASK_SERVO_OFF`、`TASK_SERVO_REBOOT`、`TASK_CLEAR_FAULT`、`TASK_REMOTE_CONTROL`、`TASK_SET_HOME`、`TASK_TEST_JOINT`、`TASK_READY_STATE`、`TASK_WALK`、`TASK_RUN`、`TASK_CLIMB_UP`），具体数值见 [任务描述](/fourier-grx-N1/docs/tasks)。`fourier_grx.JointControlMode` 提供关节控制模式常量。

### 状态字典（state dict）

Fourier-GRX Developer 接口使用状态字典（state dict）返回机器人当前的状态信息，状态字典的 key 和 value 如下：

| key                    | 说明               | 数据类型                           | 单位    | 具体描述                                                        |
|------------------------|------------------|--------------------------------|-------|-------------------------------------------------------------|
| `imu_quat`             | 机器人 IMU 的四元数姿态信息 | array(float,float,float,float) |       | x, y, z, w                                                  |
| `imu_euler_angle`      | 机器人 IMU 的欧拉角姿态信息 | array(float, float, float)     | rad   | roll, pitch, yaw                                            |
| `imu_angular_velocity` | 机器人 IMU 的角速度信息   | array(float, float, float)     | rad/s | roll, pitch, yaw                                            |
| `imu_acceleration`     | 机器人 IMU 的线加速度信息  | array(float, float, float)     | m/s^2 | x, y, z                                                     |
| `joint_position`       | 机器人关节的位置信息       | array(float * num_of_joints)   | rad   | 参考 [机器人关节序列](/fourier-grx-N1/docs/reference/joint_sequence) |
| `joint_velocity`       | 机器人关节的速度信息       | array(float * num_of_joints)   | rad/s | 参考 [机器人关节序列](/fourier-grx-N1/docs/reference/joint_sequence) |
| `joint_effort`         | 机器人关节的力矩信息       | array(float * num_of_joints)   | Nm    | 参考 [机器人关节序列](/fourier-grx-N1/docs/reference/joint_sequence) |
| `joint_current`        | 机器人关节的电流信息       | array(float * num_of_joints)   | A     | 参考 [机器人关节序列](/fourier-grx-N1/docs/reference/joint_sequence) |

### 控制字典（control dict）

Fourier-GRX Developer 接口使用控制字典（control dict）发送机器人的控制指令，控制字典的 key 和 value 如下：

| key                   | 说明                | 数据类型                         | 单位    | 具体描述                                                        |
|-----------------------|-------------------|------------------------------|-------|-------------------------------------------------------------|
| `control_mode`        | 机器人的控制模式          | int (float * num_of_joints)  |       | 0: 无控制，1: 电流控制，2: 力矩控制，3: 速度控制，4: 位置控制，6: PD 控制，9: 位置伪 PD 控制，10: 其他 |
| `position`            | 机器人关节的位置指令        | array(float * num_of_joints) | rad   | 参考 [机器人关节序列](/fourier-grx-N1/docs/reference/joint_sequence) |
| `velocity`            | 机器人关节的速度指令        | array(float * num_of_joints) | rad/s | 参考 [机器人关节序列](/fourier-grx-N1/docs/reference/joint_sequence) |
| `effort`              | 机器人关节的力矩指令        | array(float * num_of_joints) | Nm    | 参考 [机器人关节序列](/fourier-grx-N1/docs/reference/joint_sequence) |
| `current`             | 机器人关节的电流指令        | array(float * num_of_joints) | A     | 参考 [机器人关节序列](/fourier-grx-N1/docs/reference/joint_sequence) |
| `position_control_kp` | 机器人关节位置控制的 P 系数   | array(float * num_of_joints) |       | 参考 [机器人关节序列](/fourier-grx-N1/docs/reference/joint_sequence) |
| `velocity_control_kp` | 机器人关节速度控制的 P 系数   | array(float * num_of_joints) |       | 参考 [机器人关节序列](/fourier-grx-N1/docs/reference/joint_sequence) |
| `velocity_control_ki` | 机器人关节速度控制的 I 系数   | array(float * num_of_joints) |       | 参考 [机器人关节序列](/fourier-grx-N1/docs/reference/joint_sequence) |
| `pd_control_kp`       | 机器人关节 PD 控制的 P 系数 | array(float * num_of_joints) |       | 参考 [机器人关节序列](/fourier-grx-N1/docs/reference/joint_sequence) |
| `pd_control_kd`       | 机器人关节 PD 控制的 D 系数 | array(float * num_of_joints) |       | 参考 [机器人关节序列](/fourier-grx-N1/docs/reference/joint_sequence) |
