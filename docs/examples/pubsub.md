---
layout: default
title: PubSub 接口
nav_order: 2.3
parent: 示例代码
toc: true          # 启用目录
toc_min_header: 2  # 最小显示标题层级（如 H2）
toc_max_header: 3  # 最大显示标题层级（如 H3）
---

# PubSub 接口

Fourier-GRX PubSub 接口使用 [Zenoh](https://zenoh.io/) 协议与机器人通信，
接口协议类似 Developer API，
提供了更为灵活的消息发布和订阅机制进行接口通信，
适用于远端控制或对开发灵活性有更高要求的应用场景。

您可以在机器人主控电脑或任何与机器人同一局域网的计算机上运行这些示例。

> ⚠️ **注意**：
>
> 使用 PubSub 接口，其数据是通过有线/无线网络进行传输的，其延时和可靠性会受到网络质量的影响。
> 因此，用户需要根据实际情况进行调试和测试，以确保控制的稳定性和可靠性。

## 运行方法

1. 在机器人主控电脑上配置 `fourier-grx` 运行在 `服务器模式`
   ```
   # 修改 fourier-grx 配置
   fourier-grx config
   
   # 选择运行模式为服务器模式
   ```

2. 在机器人主控电脑上启动 Fourier-GRX 主程序：
    ```bash
    # 运行 `fourier-grx` 主程序
    fourier-grx start
    ```

3. 运行示例程序（可在远程电脑上执行）：
    ```bash
    # 激活 conda 环境
    conda activate fourier-grx
   
    # 运行具体机型的示例程序
    python $HOME/Wiki-GRx-Deploy/pubsub/demo_{示例名称}.py
    ```

## 接口示例

### 系统控制示例

| 示例名称  | 说明        | 代码路径                            |
|-------|-----------|---------------------------------|
| 机器人使能 | 控制机器人使能状态 | `pubsub/demo_servo_on.py`       |
| 机器人失能 | 控制机器人失能状态 | `pubsub/demo_servo_off.py`      |
| 状态监控  | 打印机器人状态信息 | `pubsub/demo_print_state.py`    |

### 运动控制示例

| 示例名称 | 说明     | 代码路径                         |
|------|--------|------------------------------|
| 准备姿态 | 进入准备状态 | `pubsub/demo_ready_state.py` |
| 行走控制 | 手柄控制行走 | `pubsub/demo_walk.py`        |
