---
layout: default
title: User 接口
nav_order: 2.1
parent: 示例代码
toc: true          # 启用目录
toc_min_header: 2  # 最小显示标题层级（如 H2）
toc_max_header: 3  # 最大显示标题层级（如 H3）
---

# User 接口

Fourier-GRX User 接口使用 [Zenoh](https://zenoh.io/) 协议与机器人通信，
适用于高层应用开发。

您可以在机器人主控电脑或任何与机器人同一局域网的计算机上运行这些示例。

## 运行方法

1. 在机器人主控电脑上配置 `fourier-grx` 运行在 `开发者模式`
   ```
   # 修改 fourier-grx 配置
   fourier-grx config
   
   # 选择运行模式为开发者模式
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
    python $HOME/Wiki-GRx-Deploy/user/demo_{示例名称}.py
    ```

### 最佳实践

- 建议使用多个终端窗口同时运行和监控程序
- 远程控制时请确保网络连接稳定
- 运行示例前请仔细阅读相关安全说明

![终端示例](/fourier-grx-N1/assets/images/example_user_terminal.png)

## 接口示例

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
