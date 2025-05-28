---
layout: default
title: 将机器人作为移动平台使用
nav_order: 3.3
parent: 参考指南
has_toc: true
---

# 将机器人作为移动平台使用

部分用户常见的一个使用需求是将机器人作为移动平台使用，来完成一些特定的任务。比如在实验室内进行巡逻、在工厂内进行物品搬运等。

这种情况下，可能用户希望能够自主控制机器人的上半身的关节运动，而下半身又需要保持稳定，且能够及时站立或是行走。

## 下半身作为移动平台控制实现

在 Fourier-GRX SDK 中，用户可以通过修改机器人的配置文件来完成下半身作为移动平台的控制实现。

1. 创建机器人的启动配置文件：
    - 打开文件夹 `~/fourier-grx/config/n1` 文件夹，找到 `config_N1__release.yaml` 文件。
    - 将该文件复制一份，命名为 `config_N1__mobile.yaml`。

2. 修改配置文件：
    - 打开 `config_N1__mobile.yaml` 文件。
    - 找到 `actuator` 字段下的 `comm_enable` 字段，将其修改为以下内容：

    ```yaml
    actuator:
        comm_enable: [
      # left leg
      true, true, true, true, true, true, 
      # right leg
      true, true, true, true, true, true,
      # waist
      true,
      # left arm
      false, false, false, false, false,
      # right arm
      false, false, false, false, false
    ]
    ```

    - 该配置表示左腿、右腿、腰部的所有关节都启用通信，左臂和右臂的关节则不启用通信。

3. 修改启动脚本：
    - 打开文件夹 `~/fourier-grx`，找到 `run.sh` 文件。
    - 找到 `run_type` 字段，将其修改为 `mobile`：

   ```bash
   run_type="mobile"
   ```

> **注意**：
>
> 如果后续希望机器人恢复为正常的操作模式，可以通过 `fourier-grx config` 命令来修改配置文件，或是修改 `run.sh` 文件中的 `run_type` 字段为原来的值。

4. 启动机器人：

   ```bash
   fourier-grx start
   ```

5. 手柄控制：
    - 参考： https://github.com/FFTAI/Wiki-GRx-Deploy/blob/FourierN1/user/demo_walk.py
    - 手柄遥控程序可以参考 fourier-grx SDK user 接口中的 demo_walk.py 程序。

   ```bash
   # 构建消息
    message = {
        "robot_task_command": fourier_grx.TaskCommand.TASK_WALK,
        "flag_task_command_update": True,
    }
   ```

## 上半身进行关节层面控制