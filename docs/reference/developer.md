---
layout: default
title: Developer 接口
nav_order: 3.2
parent: 参考指南
has_toc: true
---

# 参考指南 Developer 接口

* TOC
{:toc}

developer 接口是针对开发者底层开发提供的二次开发接口。需要在 python 环境中安装 fourier_core 和 fourier_grx 库后，直接使用库函数进行开发调用。

## 状态字典（state dict）

Fourier-GRX developer 接口使用状态字典（state dict）返回机器人当前的状态信息，状态字典的 key 和 value 如下：

| key                    | 说明               | 数据类型                           | 单位    | 具体描述             |
|------------------------|------------------|--------------------------------|-------|------------------|
| `imu_quat`             | 机器人 IMU 的四元数姿态信息 | array(float,float,float,float) |       | x, y, z, w       |
| `imu_euler_angle`      | 机器人 IMU 的欧拉角姿态信息 | array(float, float, float)     | deg   | roll, pitch, yaw |
| `imu_angular_velocity` | 机器人 IMU 的角速度信息   | array(float, float, float)     | deg/s | roll, pitch, yaw |
| `imu_acceleration`     | 机器人 IMU 的线加速度信息  | array(float, float, float)     | m/s^2 | x, y, z          |
| `joint_position`       | 机器人关节的位置信息       | array(float * num_of_joints)   | deg   | 参考机器人关节序列        |
| `joint_velocity`       | 机器人关节的速度信息       | array(float * num_of_joints)   | deg/s | 参考机器人关节序列        |
| `joint_kinetic`        | 机器人关节的力矩信息       | array(float * num_of_joints)   | Nm    | 参考机器人关节序列        |

## 控制字典（control dict）

Fourier-GRX developer 接口使用控制字典（control dict）发送机器人的控制指令，控制字典的 key 和 value 如下：

| key                   | 说明                | 数据类型                         | 单位    | 具体描述                                             |
|-----------------------|-------------------|------------------------------|-------|--------------------------------------------------|
| `control_mode`        | 机器人的控制模式          | int (float * num_of_joints)  |       | 0: 无控制，1: 电流控制，2: 力矩控制，3: 速度控制，4: 位置控制, 6: PD 控制 |
| `position`            | 机器人关节的位置指令        | array(float * num_of_joints) | deg   | 参考机器人关节序列                                        |
| `velocity`            | 机器人关节的速度指令        | array(float * num_of_joints) | deg/s | 参考机器人关节序列                                        |
| `effort`              | 机器人关节的力矩指令        | array(float * num_of_joints) | Nm    | 参考机器人关节序列                                        |
| `current`             | 机器人关节的电流指令        | array(float * num_of_joints) | A     | 参考机器人关节序列                                        |
| `position_control_kp` | 机器人关节位置控制的 P 系数   | array(float * num_of_joints) |       | 参考机器人关节序列                                        |
| `velocity_control_kp` | 机器人关节速度控制的 P 系数   | array(float * num_of_joints) |       | 参考机器人关节序列                                        |
| `velocity_control_ki` | 机器人关节速度控制的 I 系数   | array(float * num_of_joints) |       | 参考机器人关节序列                                        |
| `pd_control_kp`       | 机器人关节 PD 控制的 P 系数 | array(float * num_of_joints) |       | 参考机器人关节序列                                        |
| `pd_control_kd`       | 机器人关节 PD 控制的 D 系数 | array(float * num_of_joints) |       | 参考机器人关节序列                                        |
