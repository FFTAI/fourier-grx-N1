---
layout: default
title: 站立/行走/跑步
nav_order: 4.21
parent: 任务描述
has_toc: true
---

# 站立/行走/跑步

## 任务信息

任务 ID: `4022` / `966`

任务描述：

- 机器人通过给定速度指令，执行站立、行走或跑步的动作。
    - 速度指令包含 lin_vel_x、lin_vel_y、ang_vel_z 三个分量。
        - lin_vel_x 表示前进速度
        - lin_vel_y 表示侧向速度
        - ang_vel_z 表示转向速度
    - lin_vel_x 和 lin_vel_y 的模在 [-0.1, 0.1] m/s 范围内时，机器人执行站立动作。
    - ang_vel_z 在 [-0.1, 0.1] rad/s 范围内时，机器人执行站立动作。
    - lin_vel_x 和 lin_vel_y 的模在 [-0.5, 0.75] m/s 或 ang_vel_z 在 [-1.0, 1.0] rad/s 时，机器人执行行走动作。
    - lin_vel_x 和 lin_vel_y 的模大于 0.75 m/s 时，机器人执行跑步动作。

任务参数：

| 接口参数 | 类型      | 默认值 | 参数范围        | 描述                  |
|------|---------|-----|-------------|---------------------|
| 前进速度 | `float` | 0.0 | [-0.5, 0.5] | 机器人前进的速度，单位为 m/s。   |
| 侧向速度 | `float` | 0.0 | [-0.5, 1.5] | 机器人侧向移动的速度，单位为 m/s。 |
| 转向速度 | `float` | 0.0 | [-1.0, 1.0] | 机器人转向的速度，单位为 rad/s。 |

涉及指令接口：

- 优先级：
    - 虚拟面板 > 虚拟手柄 > 物理手柄

- 映射到虚拟手柄

| 接口参数 | 接口映射关系                               |
|------|--------------------------------------|
| 前进速度 | `grx.virtual_joystick_axis_left` y轴  |
| 侧向速度 | `grx.virtual_joystick_axis_left` x轴  |
| 转向速度 | `grx.virtual_joystick_axis_right` y轴 |

- 映射到虚拟面板

| 接口参数 | 接口映射关系                              |
|------|-------------------------------------|
| 前进速度 | `grx.virtual_panel_command_param_1` |
| 侧向速度 | `grx.virtual_panel_command_param_2` |
| 转向速度 | `grx.virtual_panel_command_param_3` |

涉及状态接口：

| 接口参数 | 接口映射关系 |
|------|--------|
|      |        |

## 更新日志
