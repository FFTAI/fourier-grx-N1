---
layout: default
title: 作为移动平台使用
nav_order: 6.1
parent: 用户案例
has_toc: true
---

# 作为移动平台使用

* TOC
{:toc}

部分用户常见的一个使用需求是将机器人作为移动平台使用，来完成一些特定的任务。比如在实验室内进行巡逻、在工厂内进行物品搬运等。

这种情况下，可能用户希望能够自主控制机器人的上半身的关节运动，而下半身又需要保持稳定，且能够及时站立或是行走。

由于这个需求比较特殊，需要既用到机器人的高层控制接口 （[User 接口](/fourier-grx-N1/docs/reference/user)）来控制机器人的下半身关节部分的运动，又要用到机器人的关节层面底层控制接口（[Developer 接口](/fourier-grx-N1/docs/reference/developer)）来控制机器人的上半身的关节运动。
因此，我们在这里提供一个使用指南，供用户参考完成该需求。

> ℹ️ **说明**：
>
> 这里本质上是将机器人的关节的运动控制分区启动并完成控制过程。

## 下半身作为移动平台控制实现

在 Fourier-GRX SDK 中，用户可以通过修改机器人的配置文件来完成下半身作为移动平台的控制实现。

1. 创建机器人的启动配置文件：
    - 打开文件夹 `~/fourier-grx/config/n1` 文件夹，找到 `config_N1__release.yaml` 文件。
    - 将该文件复制一份，命名为 `config_N1__mobile.yaml`。

2. 修改配置文件：
    - 打开 `config_N1__mobile.yaml` 文件。
    - 找到 `actuator` 字段下的 `comm_enable` 字段，将其修改为以下内容：
        - 该配置表示在使用该配置文件启动 `fourier-grx` 时，左腿、右腿、腰部的所有关节都启用通信，左臂和右臂的关节则不启用通信。

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


3. 修改启动脚本：
    - 打开文件夹 `~/fourier-grx`，找到 `run.sh` 文件。
    - 找到 `run_type` 字段，将其修改为 `mobile`：

   ```bash
   run_type="mobile"
   ```

   > ⚠️ **注意**：
   >
   > 如果后续希望机器人恢复为正常的操作模式，可以通过 `fourier-grx config` 命令来修改配置文件，或是修改 `run.sh` 文件中的 `run_type` 字段为原来的值。

4. 启动机器人：

    ```bash
    # 在 终端1 中执行以下命令
    fourier-grx start
    ```

   > ℹ️ **说明**：
   >
   > 启动程序如果不希望修改 fourier-grx 的 run.sh 脚本，也可以通过 `--config` 参数指定配置文件来启动机器人。如：
   > `python run.py --config=config_N1__mobile.yaml`。

5. 手柄控制：
    - 手柄遥控程序可以参考 fourier-grx SDK **User** 接口中的 [demo_walk.py](https://github.com/FFTAI/Wiki-GRx-Deploy/blob/FourierN1/user/demo_walk.py) 程序。

    ```bash
    # 在 终端2 中执行以下命令
    python demo_walk.py
    ```

## 上半身进行关节层面控制实现

在 Fourier-GRX SDK 中，用户可以通过修改机器人的配置文件来完成上半身关节的控制。

1. 创建机器人的启动配置文件：
    - 打开文件夹 `~/fourier-grx/config/n1` 文件夹，找到 `config_N1__release.yaml` 文件。
    - 将该文件复制一份，命名为 `config_N1__upper.yaml`。

2. 修改配置文件：
    - 打开 `config_N1__upper.yaml` 文件。
    - 找到 `peripheral` 字段下的 `use_virtual_joystick` 字段，将其修改为以下内容：
        - 该配置表示在使用该配置文件启动 `fourier-grx` 时，关闭所有外设或虚拟外设对 `fourier-grx` 库的输入。

    ```yaml
    peripheral:
        use_joystick: false  # 关闭手柄控制
        use_keyboard: false  # 关闭键盘控制
        use_virtual_joystick: false  # 关闭虚拟手柄
        use_virtual_teleoperation: false  # 关闭虚拟遥操作
    ```

    - 找到 `sensor_usb_imu` 字段下的 `comm_enable` 字段，将其修改为以下内容:
        - 该配置表示在使用该配置文件启动 `fourier-grx` 时，USB IMU 传感器不启用通信。**（防止与下半身运动控制任务设备读取冲突）**

    ```yaml
    sensor_usb_imu:
        comm_enable: [
        false
    ]
    ```

    - 找到 `actuator` 字段下的 `comm_enable` 字段，将其修改为以下内容：
        - 该配置表示在使用该配置文件启动 `fourier-grx` 时，左臂和右臂的所有关节都启用通信，左腿、右腿和腰部的关节则不启用通信。

    ```yaml
    actuator:
        comm_enable: [
        # left leg
        false, false, false, false, false, false, 
        # right leg
        false, false, false, false, false, false,
        # waist
        false,
        # left arm
        true, true, true, true, true,
        # right arm
        true, true, true, true, true
    ]
    ```

3. 启动上肢控制程序：
    - 上肢关节控制程序可以参考 fourier-grx SDK **Developer** 接口中的 [demo_ready_state.py](https://github.com/FFTAI/Wiki-GRx-Deploy/blob/FourierN1/developer/demo_ready_state.py) 程序。
    - 启动时需指定配置文件为上肢控制专用配置文件，可以让机器人关节上肢部分移动到准备状态位置。

    ```bash
    # 在 终端3 中执行以下命令
    python demo_ready_state.py --config=config_N1__upper.yaml
    ```

## 注意事项

如果遇到问题，欢迎给我们提 [issue](https://github.com/FFTAI/Wiki-GRx-Deploy/issues)，谢谢！