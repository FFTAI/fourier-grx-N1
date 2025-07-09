---
layout: default
title: 站立/行走/跑步
nav_order: 4.21
parent: 任务描述
has_toc: true
---

# 站立/行走/跑步

## 任务信息

任务值 (TID): `4022` / `966`

任务描述：

- 机器人通过给定速度指令，执行站立、行走或跑步的动作。
    - 速度指令包含 `lin_vel_x`、`lin_vel_y`、`ang_vel_z` 三个分量。
        - `lin_vel_x` 表示前进速度
        - `lin_vel_y` 表示侧向速度
        - `ang_vel_z` 表示转向速度
    - `lin_vel_x` 和 `lin_vel_y` 的模在 [-0.1, 0.1] m/s 范围内时，机器人执行站立动作。
    - `ang_vel_z` 在 [-0.1, 0.1] rad/s 范围内时，机器人执行站立动作。
    - `lin_vel_x` 和 `lin_vel_y` 的模在 [-0.5, 0.75] m/s 或 `ang_vel_z` 在 [-1.0, 1.0] rad/s 时，机器人执行行走动作。
    - `lin_vel_x` 和 `lin_vel_y` 的模大于 0.75 m/s 时，机器人执行跑步动作。

任务参数：

| 接口参数 | 类型      | 默认值 | 参数范围        | 描述                  |
|------|---------|-----|-------------|---------------------|
| 前进速度 | `float` | 0.0 | [-0.5, 0.5] | 机器人前进的速度，单位为 m/s。   |
| 侧向速度 | `float` | 0.0 | [-0.5, 1.5] | 机器人侧向移动的速度，单位为 m/s。 |
| 转向速度 | `float` | 0.0 | [-1.0, 1.0] | 机器人转向的速度，单位为 rad/s。 |

## 模块信息

| 模块值 (MID) | 模块指令                         | 模块描述  |
|-----------|------------------------------|-------|
| 3407      | COMPONENT_NATURAL_WAVE       | 自然摆臂  |
| 2412      | COMPONENT_WAVE_LEFT_HAND     | 左手打招呼 |
| 2411      | COMPONENT_WAVE_RIGHT_HAND    | 右手打招呼 |
| 3408      | COMPONENT_RAISE_RIGHT_BOXING | 右手握拳  |
| 3409      | COMPONENT_RAISE_RIGHT_HAND   | 右手举起  |
| 3410      | COMPONENT_SPREAD_HAND        | 双手张开  |

## 接口信息

状态接口：

| 接口参数 | 接口映射关系 |
|------|--------|
|      |        |

指令接口：

- 优先级：
    - 虚拟面板 > 虚拟键盘 > 虚拟手柄 > 物理键盘 > 物理手柄

- 映射到物理手柄

| 接口参数 | 接口映射关系                                       |
|------|----------------------------------------------|
| 前进速度 | `joystick_axis_left` y轴，映射到 [min, max] 速度范围  |
| 侧向速度 | `joystick_axis_left` x轴，映射到 [min, max] 速度范围  |
| 转向速度 | `joystick_axis_right` y轴，映射到 [min, max] 速度范围 |

- 映射到物理键盘

| 接口参数 | 接口映射关系                                                   |
|------|----------------------------------------------------------|
| 前进速度 | `keyboard_key_w` `keyboard_key_s`，映射到 [-0.25, 0.25] 速度范围 |
| 侧向速度 | `keyboard_key_a` `keyboard_key_d`，映射到 [-0.25, 0.25] 速度范围 |
| 转向速度 | `keyboard_key_q` `keyboard_key_e`，映射到 [-0.25, 0.25] 速度范围 |

- 映射到虚拟手柄

| 接口参数 | 接口映射关系                                                   |
|------|----------------------------------------------------------|
| 前进速度 | `grx.virtual_joystick_axis_left` y轴，映射到 [min, max] 速度范围  |
| 侧向速度 | `grx.virtual_joystick_axis_left` x轴，映射到 [min, max] 速度范围  |
| 转向速度 | `grx.virtual_joystick_axis_right` y轴，映射到 [min, max] 速度范围 |

- 映射到虚拟键盘

| 接口参数 | 接口映射关系                                                                           |
|------|----------------------------------------------------------------------------------|
| 前进速度 | `grx.virtual_keyboard_key_w` `grx.virtual_keyboard_key_s`，映射到 [-0.25, 0.25] 速度范围 |
| 侧向速度 | `grx.virtual_keyboard_key_a` `grx.virtual_keyboard_key_d`，映射到 [-0.25, 0.25] 速度范围 |
| 转向速度 | `grx.virtual_keyboard_key_q` `grx.virtual_keyboard_key_e`，映射到 [-0.25, 0.25] 速度范围 |

- 映射到虚拟面板

| 接口参数 | 接口映射关系                              |
|------|-------------------------------------|
| 前进速度 | `grx.virtual_panel_command_param_1` |
| 侧向速度 | `grx.virtual_panel_command_param_2` |
| 转向速度 | `grx.virtual_panel_command_param_3` |

## 更新日志
