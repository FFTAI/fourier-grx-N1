---
layout: default
title: PubSub 接口
nav_order: 3.3
parent: 参考指南
has_toc: true
---

# 参考指南 PubSub 接口

* TOC
{:toc}

> ℹ️ **说明**：
> 
> 使用 Fourier-GRX-N1 SDK PubSub API 前，请将 `fourier-grx` 配置为 **服务器模式**。

Fourier-GRX PubSub 接口使用 zenoh 进行通信，zenoh 是一个分布式系统的数据共享和协作平台 (https://zenoh.io/)。

PubSub 接口是基于 Developer 接口内容，借助于 zenoh 的发布/订阅机制实现的。它允许用户通过订阅特定的主题 (topic) 来接收机器人状态信息，并通过发布特定的主题来发送控制指令。

PubSub 接口主要分为以下4类：

- `robot`：机器人相关信息
- `task`：任务相关信息

对应的 zenoh 接口 topic 的 key 格式为：

- key 可以是 ["robot", "task"]
- `fourier-grx/{key}/state` ：机器人主控程序发出的状态信息 **[机器人作为服务器]**
- `fourier-grx/{key}/control` ：机器人主控程序接收的指令信息 **[用户机作为客户端]**

---

## 状态信息

### robot/state 接口协议 (状态信息)

key 说明列表：

| key                    | 说明         | 数据类型                              | 具体描述                                    |
|------------------------|------------|-----------------------------------|-----------------------------------------|
| `imu_quat`             | 传感器 IMU 数据 | array(float, float, float, float) | 传感器 IMU 数据，四元数表示姿态，顺序为 x, y, z, w       |
| `imu_euler_angle`      | 传感器 IMU 数据 | array(float, float, float)        | 传感器 IMU 数据，欧拉角表示姿态，顺序为 roll, pitch, yaw |
| `imu_angular_velocity` | 传感器 IMU 数据 | array(float, float, float)        | 传感器 IMU 数据，角速度表示姿态，顺序为 x, y, z          |
| `imu_acceleration`     | 传感器 IMU 数据 | array(float, float, float)        | 传感器 IMU 数据，线加速度表示姿态，顺序为 x, y, z         |

| key              | 说明   | 数据类型         | 具体描述           |
|------------------|------|--------------|----------------|
| `joint_position` | 关节位置 | array(float) | 关节测量位置，单位为角度   |
| `joint_velocity` | 关节速度 | array(float) | 关节测量速度，单位为角度/秒 |
| `joint_effort`   | 关节力矩 | array(float) | 关节测量力矩，单位为牛顿米  |
| `joint_current`  | 关节电流 | array(float) | 关节测量电流，单位为安培   |

### task/state 接口协议 (状态信息)

key 说明列表：

| key                 | 说明   | 数据类型 | 具体描述           |
|---------------------|------|------|----------------|
| `task_command`      | 任务指令 | int  | 设置机器人任务值 (TID) |
| `component_command` | 模块指令 | int  | 设置机器人模块值 (MID) |

---

## 指令信息

### robot/control 接口协议 (指令信息)

key 说明列表：

| key            | 说明    | 数据类型         | 具体描述                                                                        |
|----------------|-------|--------------|-----------------------------------------------------------------------------|
| `group_name`   | 关节组名称 | string       | 关节组名称，目前只支持 `whole_body`                                                    |
| `control_mode` | 控制模式  | int          | 0: 无控制，1: 电流控制，2: 力矩控制，3: 速度控制，4: 位置控制, 6: PD 控制                            |
| `kp`           | P 系数  | array(float) | 控制的 P 系数，单位为无量纲，参考 [机器人关节序列](/fourier-grx-N1/docs/reference/joint_sequence) |
| `kd`           | D 系数  | array(float) | 控制的 D 系数，单位为无量纲，参考 [机器人关节序列](/fourier-grx-N1/docs/reference/joint_sequence) |
| `position`     | 位置指令  | array(float) | 关节位置指令，单位为角度，参考 [机器人关节序列](/fourier-grx-N1/docs/reference/joint_sequence)    |

### task/control 接口协议 (指令信息)

key 说明列表：

| key                 | 说明   | 数据类型 | 具体描述           |
|---------------------|------|------|----------------|
| `task_execute`      | 任务状态 | int  | 当前机器人任务值 (TID) |
| `component_execute` | 模块状态 | int  | 当前机器人模块值 (MID) |
