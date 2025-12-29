---
layout: default
title: 机器人关节序列
nav_order: 3.4
parent: 参考指南
has_toc: true
---

# 机器人关节序列

* TOC
{:toc}

在获取关节信息或是发送控制指令时，需要整体发送所有关节的信息。因此需要按照机器人的关节序列进行操作。

Fourier-GRX SDK 提供的关节序列与机器人 URDF 模型文件中的关节序列一致。用户在使用时需要注意关节序列的顺序。

Fourier-GRX SDK 中对于关节序列的定义如下。

## N1

- 总数：6 + 6 + 1 + 0 + 5 + 5 = 23
- 左腿：6
- 右腿：6
- 腰部：1
- 头部：0
- 左臂：5
- 右臂：5

| 序号 | 关节名称                       | 具体描述        | IP 地址          |
|----|----------------------------|-------------|----------------|
| 0  | left_hip_pitch_joint       | 左髋 pitch 关节 | 192.168.137.70 |
| 1  | left_hip_roll_joint        | 左髋 roll 关节  | 192.168.137.71 |
| 2  | left_hip_yaw_joint         | 左髋 yaw 关节   | 192.168.137.72 |
| 3  | left_knee_pitch_joint      | 左膝 pitch 关节 | 192.168.137.73 |
| 4  | left_ankle_roll_joint      | 左踝 roll 关节  | 192.168.137.74 |
| 5  | left_ankle_pitch_joint     | 左踝 pitch 关节 | 192.168.137.75 |
| 6  | right_hip_pitch_joint      | 右髋 pitch 关节 | 192.168.137.50 |
| 7  | right_hip_roll_joint       | 右髋 roll 关节  | 192.168.137.51 |
| 8  | right_hip_yaw_joint        | 右髋 yaw 关节   | 192.168.137.52 |
| 9  | right_knee_pitch_joint     | 右膝 pitch 关节 | 192.168.137.53 |
| 10 | right_ankle_roll_joint     | 右踝 roll 关节  | 192.168.137.54 |
| 11 | right_ankle_pitch_joint    | 右踝 pitch 关节 | 192.168.137.55 |
| 12 | waist_yaw_joint            | 腰部 yaw 关节   | 192.168.137.90 |
| 13 | left_shoulder_pitch_joint  | 左肩 pitch 关节 | 192.168.137.10 |
| 14 | left_shoulder_roll_joint   | 左肩 roll 关节  | 192.168.137.11 |
| 15 | left_shoulder_yaw_joint    | 左肩 yaw 关节   | 192.168.137.12 |
| 16 | left_elbow_pitch_joint     | 左肘 pitch 关节 | 192.168.137.13 |
| 17 | left_wrist_yaw_joint       | 左腕 yaw 关节   | 192.168.137.14 |
| 18 | right_shoulder_pitch_joint | 右肩 pitch 关节 | 192.168.137.30 |
| 19 | right_shoulder_roll_joint  | 右肩 roll 关节  | 192.168.137.31 |
| 20 | right_shoulder_yaw_joint   | 右肩 yaw 关节   | 192.168.137.32 |
| 21 | right_elbow_pitch_joint    | 右肘 pitch 关节 | 192.168.137.33 |
| 22 | right_wrist_yaw_joint      | 右腕 yaw 关节   | 192.168.137.34 |

