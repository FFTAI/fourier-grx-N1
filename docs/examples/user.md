---
layout: default
title: User 接口
nav_order: 2.1
parent: 示例代码
toc: true          # 启用目录
toc_min_header: 2  # 最小显示标题层级（如 H2）
toc_max_header: 3  # 最大显示标题层级（如 H3）
---

# 示例代码 User 接口

User 接口提供的开发示例有：

- `demo_servo_on`: 机器人全关节上电使能。
- `demo_servo_off`: 机器人全关节下电失能。
- `demo_clear_fault`: 清除机器人全关节报警。当机器人出现报警时，可以通过此接口清除报警。
- `demo_set_home`: 设置机器人全关节零位位置为当前位置，用于标定机器人关节零位。
- `demo_test_joint`: 机器人关节运动功能测试，用于检测机器人关节是否能够正常运动。
- `demo_ready_state`: 机器人运行到 **准备状态**，为微曲膝关节的站立姿态。
- `demo_walk`: 机器人运动到 **行走状态**，可以用手柄控制机器人行走。

## demo_walk

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