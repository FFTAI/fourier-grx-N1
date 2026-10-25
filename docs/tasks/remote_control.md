---
layout: default
title: 远程控制
nav_order: 5.08
parent: 任务描述
has_toc: true
---

# 远程控制

## 任务信息

任务值 (TID): **43**

任务描述：

- 机器人释放内部关节控制（将所有关节目标控制模式设为无控制），交由外部接口接管。
- 供 Developer / PubSub 接口进行直接关节控制前使用，避免内置运动任务与外部指令同时控制关节。
- 执行后机器人自动回到空闲任务。

任务参数：

| 接口参数 | 类型 | 默认值 | 参数范围 | 描述 |
|------|----|-----|------|----|
|      |    |     |      |    |

## 模块信息

| 模块值 (MID) | 模块指令 | 模块描述 | 涉及关节 |
|-----------|------|------|------|
|           |      |      |

## 接口信息

状态接口：

- 任务指令：`task.robot_task_command` 发送 `43`（`fourier_grx.TaskCommand.TASK_REMOTE_CONTROL`），`task.flag_task_command_update` 发送 `1` 确认。
- 开发者接口：`control_system.robot_control_set_task_command(43)`。

## 补充说明

- PubSub 示例中，机器人通过 PubSub 接口接收 `robot/control` 关节指令前，先发送该任务释放内部关节控制。
- 该任务不改变执行器使能状态；如需保持姿态，请配合 [固定位置](/fourier-grx-N1/docs/tasks/fix_position) 或外部 PD 控制使用。