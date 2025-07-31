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

用户接口通过 [Zenoh](https://zenoh.io/) 协议与机器人通信，适用于高层应用开发。您可以在任何与机器人同一局域网的计算机上运行这些示例。

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

### demo_walk

该示例代码展示如何通过调用 fourier-grx 内部 RL 行走算法完成机器人运动控制任务。

```python
import time
import zenoh
import msgpack
import pygame
import threading

import fourier_grx.sdk.user as fourier_grx

joystick = None
axis_left = (0.0, 0.0)
axis_right = (0.0, 0.0)


def demo_task():
    global joystick, axis_left, axis_right

    prefix = "fourier-grx"

    # 初始化 zenoh 会话
    zenoh_config = zenoh.Config()
    zenoh_session: zenoh.Session = zenoh.open(zenoh_config)

    # 构建发布者
    zenoh_task_publisher = zenoh_session.declare_publisher(
        key_expr=f"{prefix}/dynalink_interface/task/client",  # 目标发布者的 key 表达式
        priority=zenoh.Priority.REAL_TIME,
        congestion_control=zenoh.CongestionControl.DROP,
    )
    zenoh_grx_publisher = zenoh_session.declare_publisher(
        key_expr=f"{prefix}/dynalink_interface/grx/client",  # 目标发布者的 key 表达式
        priority=zenoh.Priority.REAL_TIME,
        congestion_control=zenoh.CongestionControl.DROP,
    )

    # 构建消息
    message = {
        "robot_task_command": fourier_grx.TaskCommand.TASK_WALK,
        "flag_task_command_update": True,
    }

    print("Sending message: ", message)

    # 发布消息
    zenoh_task_publisher.put(msgpack.packb(message))

    # 等待 1s (确保消息被发送)
    time.sleep(1)

    # 创建子线程, 用于监听摇杆输入
    pygame.init()
    pygame.joystick.init()

    joystick = pygame.joystick.Joystick(0)
    joystick.init()

    thread_joystick_listener = threading.Thread(target=joystick_listener)
    thread_joystick_listener.start()

    # 等待 1s (确保摇杆监听线程启动)
    time.sleep(1)

    # 用摇杆控制机器人走路速度
    try:
        while True:
            # 构建消息
            message = {
                "virtual_joystick_axis_left": axis_left,
                "virtual_joystick_axis_right": axis_right,
            }

            # 发布消息
            zenoh_grx_publisher.put(msgpack.packb(message))

            # 等待 0.02s, 以减少 CPU 占用
            time.sleep(0.02)
    except KeyboardInterrupt:
        pass
    finally:
        pass

    # 关闭 zenoh 会话
    zenoh_session.close()

    # 关闭摇杆
    pygame.quit()


def joystick_listener():
    global joystick, axis_left, axis_right

    while True:
        pygame.event.get()

        # 获取摇杆输入
        axis_left = joystick.get_axis(0), joystick.get_axis(1)
        axis_right = joystick.get_axis(3), 0

        # 等待 0.02s, 以减少 CPU 占用
        time.sleep(0.02)


if __name__ == "__main__":
    demo_task()


```