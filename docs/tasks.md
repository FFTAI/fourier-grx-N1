---
layout: default
title: 任务描述
nav_order: 4
---

# 任务描述

* TOC
{:toc}

> ⚠️ **注意**：
>
> 本章中未列出的任务功能，在使用手柄进行机器人控制，选择任务时仍可能会查看到，表明该任务可能仍处于开发中，或部份已废弃使用（未及时删去）。
> 因此，建议开发者在使用手柄进行机器人操作时，谨慎选择本章中未列出的任务进行执行，推荐只尝试本技术文档中 [任务描述](/fourier-grx-N1/docs/tasks) 部分中列出的任务指令。

## 任务和模块

机器人任务指令列表 🎏：

- 任务指令通过 `fourier_grx.TaskCommand` 枚举类定义，具体定义如下：
- 具体 `任务值 (TID)` 可能会根据机器人的不同，或 `fourier-grx` 版本的不同而有所变化，**具体值以实际任务信息为准**。

N1 机器人任务指令示例：

| 任务指令              | 任务值 (TID) | 适用机型 | 任务描述                            |
|-------------------|-----------|------|---------------------------------|
| TASK_SERVO_OFF    | 36        | All  | 机器人全关节下电失能                      |
| TASK_SERVO_ON     | 35        | All  | 机器人全关节上电使能                      |
| TASK_SERVO_REBOOT | 41        | All  | 机器人全关节上电使能并重启                   |
| TASK_CLEAR_FAULT  | 34        | All  | 清除机器人全关节报警                      |
| TASK_SET_HOME     | 999       | All  | 设置机器人全关节零位位置为当前位置               |
| TASK_TEST_JOINT   | 902       | All  | 机器人关节运动功能测试，用于检测机器人关节是否能够正常运动   |
| TASK_READY_STATE  | 960       | All  | 机器人运行到 **准备状态**，为微曲膝关节的站立姿态     |
| TASK_WALK         | 965       | All  | 机器人运动到 **行走状态**，可以用手柄控制机器人行走    |
| TASK_RUN          | 966       | N1   | 机器人运动到 **跑步状态**，可以用手柄控制机器人行走/跑步 |

- All：表示所有傅利叶智能生产机型
- N1：表示傅利叶 N1 系列机型

---

机器人模块指令列表 🎏：

- 模块任务可以理解为任务下面的子任务模块，但是由于子任务之间可能存在 **互斥、组合** 的关系，因此，我们并不称其为子任务，而是以 **模块** 的形式进行管理。
- 相互互斥的几个模块其中一个被调用时，另一个在程序中自动被 挂起。（由控制程序自动管理）
- 相互组合的模块，对方的调用和切换不会互相影响。
- 模块的运行管理全在控制程序中完成，上层无需关心模块的吊起切换过程是否有风险。
- `模块值 (MID)` 跟机型绑定关系密切，因此，可能随版本变动，如果发现模块任务未正常执行，建议及时更新到最新固件并 **以具体任务中关于模块信息的描述为准**。

N1 机器人模块指令示例：

| 任务指令      | 任务值 (TID) | 模块指令                         | 模块值 (MID) | 模块描述  |
|-----------|-----------|------------------------------|-----------|-------|
| TASK_WALK | 965       | COMPONENT_NATURAL_WAVE       | 3407      | 自然摆臂  |
| TASK_WALK | 965       | COMPONENT_WAVE_LEFT_HAND     | 2412      | 左手打招呼 |
| TASK_WALK | 965       | COMPONENT_WAVE_RIGHT_HAND    | 2411      | 右手打招呼 |
| TASK_WALK | 965       | COMPONENT_RAISE_RIGHT_BOXING | 3408      | 右手握拳  |
| TASK_WALK | 965       | COMPONENT_RAISE_RIGHT_HAND   | 3409      | 右手举起  |
| TASK_WALK | 965       | COMPONENT_SPREAD_HAND        | 3410      | 双手张开  |

| 任务指令     | 任务值 (TID) | 模块指令                         | 模块值 (MID) | 模块描述  |
|----------|-----------|------------------------------|-----------|-------|
| TASK_RUN | 966       | COMPONENT_NATURAL_WAVE       | 3407      | 自然摆臂  |
| TASK_RUN | 966       | COMPONENT_WAVE_LEFT_HAND     | 2412      | 左手打招呼 |
| TASK_RUN | 966       | COMPONENT_WAVE_RIGHT_HAND    | 2411      | 右手打招呼 |
| TASK_RUN | 966       | COMPONENT_RAISE_RIGHT_BOXING | 3408      | 右手握拳  |
| TASK_RUN | 966       | COMPONENT_RAISE_RIGHT_HAND   | 3409      | 右手举起  |
| TASK_RUN | 966       | COMPONENT_SPREAD_HAND        | 3410      | 双手张开  |

## 任务切换

在使用不同方式进行机器人控制时，任务切换的方式可能会有所不同，以下是常见的任务切换方式：

|      | 物理手柄    | 物理键盘          | 通信 User 接口                                     | 通信 Developer 接口                                                       |
|------|---------|---------------|------------------------------------------------|-----------------------------------------------------------------------|
| 任务选择 | `L1` 按键 | `up`/`down` 键 | `task.robot_task_command` 发送 `任务值 (TID)`       |                                                                       |
| 任务确认 | `L2` 按键 | `enter` 键     | `task.flag_task_command_update` 发送 1 确认更新      | `control_system.robot_control_set_task_command(TID)` 发送 `任务值 (TID)`   |
| 模块选择 | `R1` 按键 |               | `task.robot_component_command` 发送 `模块值 (MID)`  |                                                                       |
| 模块确认 | `R2` 按键 |               | `task.flag_component_command_update` 发送 1 确认更新 | `control_system.robot_control_set_task_component(MID)` 发送 `模块值 (MID)` |

