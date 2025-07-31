---
layout: default
title: 示例代码
nav_order: 2
has_toc: true
---

# 示例代码

* TOC
{:toc}

本文档提供了丰富的示例代码，帮助您快速掌握 Fourier-GRX-N1 SDK 的使用方法。

- N1 系列机器人的示例代码位于 [Github Wiki-GRx-Deploy](https://github.com/FFTAI/Wiki-GRx-Deploy.git) 的 `FourierN1` 分支中。

示例代码包含用户接口（User API）和开发者接口（Developer API）两类。

## 示例程序下载

可以通过 git 同步机器人的二次开发接口示例程序，同步命令为：

### N1 系列机器人

```bash
# 在机器人主控电脑 $HOME 目录下执行
git clone https://github.com/FFTAI/Wiki-GRx-Deploy.git --branch=FourierN1
cd $HOME/Wiki-GRx-Deploy
```

## 二次开发环境配置

`fourier-grx` 工具提供了 `fourier-grx setup_conda` 命令用于一键配置 conda 开发环境用于机器人二次开发。

```bash
# 在机器人主控电脑上：
fourier-grx setup_conda

# 程序运行完成后，会搭建出一个名为 `fourier-grx` 的 conda 环境，可以通过以下命令激活该环境
conda activate fourier-grx

# 如果希望自主搭建开发环境，可以在 $HOME/fourier-grx/whl 中找到依赖库文件进行手动安装。
```

---

## 用户接口示例（User API）

用户接口通过 [Zenoh](https://zenoh.io/) 协议与机器人通信，适用于高层应用开发。您可以在任何与机器人同一局域网的计算机上运行这些示例。

### 基础控制示例

| 示例名称  | 说明        | 代码路径                        |
|-------|-----------|-----------------------------|
| 执行器使能 | 使能机器人执行器  | `user/demo_servo_on.py`     |
| 执行器失能 | 失能机器人执行器  | `user/demo_servo_off.py`    |
| 执行器重启 | 重启机器人执行器  | `user/demo_servo_reboot.py` |
| 清除故障  | 清除机器人报警状态 | `user/demo_clear_fault.py`  |
| 设置零位  | 设置当前位置为零位 | `user/demo_set_home.py`     |

### 运动控制示例

| 示例名称 | 说明          | 代码路径                       |
|------|-------------|----------------------------|
| 关节测试 | 测试各关节运动功能   | `user/demo_test_joint.py`  |
| 准备姿态 | 进入准备状态（微曲膝） | `user/demo_ready_state.py` |
| 行走控制 | 使用手柄控制机器人行走 | `user/demo_walk.py`        |

### 运行方法

1. 在机器人主控电脑上启动 Fourier-GRX 主程序：
    ```bash
    # 激活 conda 环境
    conda activate fourier-grx
   
    # 运行具体机型的示例程序，配置文件需要根据具体机型进行修改，使用带 "_sdk.yaml" 后缀的配置文件
    python $HOME/fourier-grx/whl/run.py --config=$HOME/fourier-grx/config/{具体机型}/config_{具体机型}_sdk.yaml
    ```

2. 运行示例程序（可在远程电脑上执行）：
    ```bash
    # 激活 conda 环境
    conda activate fourier-grx
   
    # 运行具体机型的示例程序
    python $HOME/Wiki-GRx-Deploy/user/demo_{示例名称}.py
    ```

### 最佳实践

- 建议使用多个终端窗口同时运行和监控程序
- 远程控制时请确保网络连接稳定
- 运行示例前请仔细阅读相关安全说明

![终端示例](/fourier-grx-N1/assets/images/example_user_terminal.png)

---

## 开发者接口示例（Developer API）

开发者接口直接调用底层硬件接口，适用于底层开发。为确保实时性，这些示例必须在机器人主控电脑上运行。

### 系统控制示例

4.0.0 以上版本：

| 示例名称  | 说明          | 代码路径                                    |
|-------|-------------|-----------------------------------------|
| 执行器使能 | 使能机器人执行器    | `developer_radian/demo_servo_on.py`     |
| 执行器失能 | 失能机器人执行器    | `developer_radian/demo_servo_off.py`    |
| 执行器重启 | 重启机器人执行器    | `developer_radian/demo_servo_reboot.py` |
| 状态监控  | 打印机器人状态信息   | `developer_radian/demo_print_state.py`  |
| 参数配置  | 设置关节 PID 参数 | `developer_radian/demo_set_pid.py`      |
| 零位设置  | 设置机器人零位     | `developer_radian/demo_set_home.py`     |

4.0.0 以下版本：

| 示例名称  | 说明          | 代码路径                             |
|-------|-------------|----------------------------------|
| 执行器使能 | 使能机器人执行器    | `developer/demo_servo_on.py`     |
| 执行器失能 | 失能机器人执行器    | `developer/demo_servo_off.py`    |
| 执行器重启 | 重启机器人执行器    | `developer/demo_servo_reboot.py` |
| 状态监控  | 打印机器人状态信息   | `developer/demo_print_state.py`  |
| 参数配置  | 设置关节 PID 参数 | `developer/demo_set_pid.py`      |
| 零位设置  | 设置机器人零位     | `developer/demo_set_home.py`     |

### 运动控制示例

4.0.0 以上版本：

| 示例名称 | 说明     | 代码路径                                   |
|------|--------|----------------------------------------|
| 准备姿态 | 进入准备状态 | `developer_radian/demo_ready_state.py` |
| 行走控制 | 手柄控制行走 | `developer_radian/demo_walk.py`        |

4.0.0 以下版本：

| 示例名称 | 说明     | 代码路径                            |
|------|--------|---------------------------------|
| 准备姿态 | 进入准备状态 | `developer/demo_ready_state.py` |
| 行走控制 | 手柄控制行走 | `developer/demo_walk.py`        |

### 运行方法

```bash
# 激活 conda 环境
conda activate fourier-grx

# 运行具体机型的示例程序，配置文件需要根据具体机型进行修改，使用带 "_developer.yaml" 后缀的配置文件
python $HOME/Wiki-GRx-Deploy/developer/demo_{示例名称}.py --config=$HOME/fourier-grx/config/{具体机型}/config_{具体机型}_developer.yaml
```

### 注意事项

1. 开发者接口需要更深入的机器人知识
2. 建议先熟悉用户接口再使用开发者接口
3. 请注意备份重要的配置文件
4. 调试时建议在安全环境下进行

## PubSub 接口示例

开发者通过 [Zenoh](https://zenoh.io/) 协议与机器人通信，接口协议类似 Developer API，提供了更为灵活的消息发布和订阅机制进行接口通信。

> ⚠️ **注意**：
>
> 使用 PubSub 接口，其数据是通过有线/无线网络进行传输的，其延时和可靠性会受到网络质量的影响。
> 因此，用户需要根据实际情况进行调试和测试，以确保控制的稳定性和可靠性。

### 系统控制示例

| 示例名称  | 说明        | 代码路径                                   |
|-------|-----------|----------------------------------------|
| 机器人使能 | 控制机器人使能状态 | `pubsub_radian/demo_servo_on.py`       |
| 机器人失能 | 控制机器人失能状态 | `pubsub_radian/demo_servo_off.py`      |
| 状态监控  | 打印机器人状态信息 | `pubsub_radian/demo_print_state.py`    |
| 远程控制  | 远端控制器示例   | `pubsub_radian/demo_remote_control.py` |

### 运动控制示例

| 示例名称 | 说明     | 代码路径                                |
|------|--------|-------------------------------------|
| 准备姿态 | 进入准备状态 | `pubsub_radian/demo_ready_state.py` |
| 行走控制 | 手柄控制行走 | `pubsub_radian/demo_walk.py`        |
