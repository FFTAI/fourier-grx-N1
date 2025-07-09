---
layout: default
title: Sim2Real
nav_order: 5.2
parent: 模型训练
toc: true          # 启用目录
toc_min_header: 2  # 最小显示标题层级（如 H2）
toc_max_header: 3  # 最大显示标题层级（如 H3）
---

# Sim2Real

如果您在使用 N1 机器人进行 Sim2Real 部署时遇到问题，可以参考以下步骤来解决：

1. 验证开源仓库开源仓库提供的 [Wiki-GRx-Deploy](https://github.com/FFTAI/Wiki-GRx-Deploy) 中
   `FourierN1` 分支中 `developer` 文件夹下的 `demo_print_state.py` 程序是否可以完成真机部署并正常运行，打印数据一切正常。
2. 如果上述程序可以正常运行，可以说明以下问题：
    - SDK 环境配置正确。
    - IMU 外设数据采集正常。
    - 关节执行器通信以及数据采集正常。

3. 验证开源仓库开源仓库提供的 [Wiki-GRx-Deploy](https://github.com/FFTAI/Wiki-GRx-Deploy) 中
   `FourierN1` 分支中 `developer` 文件夹下的 `demo_ready_state.py` 程序是否可以完成真机部署并正常运行。
4. 如果上述程序可以正常运行，可以说明以下问题：
    - SDK 环境配置正确。
    - IMU 外设数据采集正常。
    - 关节执行器通信以及数据采集正常。
    - 关节执行器的控制指令可以正常发送并执行。

5. 验证我们在开源仓库提供的 [Wiki-GRx-Deploy](https://github.com/FFTAI/Wiki-GRx-Deploy) 中
   `FourierN1` 分支中 `developer` 文件夹下的 `demo_walk.py` 程序是否可以完成真机部署并正常运行。
6. 如果上述程序可以正常运行，可以说明以下问题：
    - SDK 环境配置正确。
    - IMU 外设数据采集正常。
    - 关节执行器通信以及数据采集正常。
    - 关节执行器的控制指令可以正常发送并执行。
    - 神经网络的计算可以正常运行。

7. 部署验证您自己的训练模型和代码，如果仍有问题，请检查以下内容：
    - 确保您的模型中涉及传感器以及执行器输入的部分内容是次序正确的：
        - IMU 数据的输入顺序应为
            - `imu_quat`: x, y, z, w
            - `imu_euler_angle`: roll, pitch, yaw，单位为度（deg）
            - `imu_angular_velocity`: roll, pitch, yaw，单位为度每秒（deg/s）
        - 执行器数据的输入顺序应为
            - `joint_position`: 参考 [机器人关节序列](/fourier-grx-N1/docs/reference/joint_sequence)，单位为度（deg）
            - `joint_velocity`: 参考 [机器人关节序列](/fourier-grx-N1/docs/reference/joint_sequence)，单位为度每秒（deg/s）
            - `joint_effort`: 参考 [机器人关节序列](/fourier-grx-N1/docs/reference/joint_sequence)，单位为牛顿米（Nm）
    - 确保您的模型中涉及执行器输出的部分内容是次序正确的：
        - 执行器输出的顺序应为
            - `control_mode`: 参考 [机器人关节序列](/fourier-grx-N1/docs/reference/joint_sequence)
            - `pd_control_kp`: 参考 [机器人关节序列](/fourier-grx-N1/docs/reference/joint_sequence)
            - `pd_control_kd`: 参考 [机器人关节序列](/fourier-grx-N1/docs/reference/joint_sequence)
            - `position`: 参考 [机器人关节序列](/fourier-grx-N1/docs/reference/joint_sequence)，单位为度（deg）
    - 确保您的仿真训练环境参数配置合理：
        - 请使用我们仿真训练代码中的关节的 `friction`, `armature`, `mechanism` 等参数。
        - 请使用我们仿真训练代码中的关节的 `stiffness`, `damping` 等参数。
        - 请使用我们仿真训练代码中的的 `noise`, `noise_scale` 等参数。
    - 请确保您的训练代码没有错误，在 RL 训练中，训练代码中有 bug 是导致 Sim2Real 失败的常见原因之一。

8. 具体到 Sim2Real 的 Gap 大小：
   - 如果您的机器人是 **乱抽乱动**，大概率是输入到神经网络的数据不正确，或是与仿真中的数据有较大差异。请 **仔细检查** 您的输入数据是否正确，是否与仿真中的数据有较大差异。
   - 如果机器人只是运动不协调，或是运动不平稳，则可能是训练过程中设计的奖赏参数或是环境参数不合适。

（如果仍然解决不了，可以联系我们，请工程师喝杯奶茶🧋，我们将尽快帮助您解决问题。）