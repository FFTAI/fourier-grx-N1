---
layout: default
title: 机器人关节序列
nav_order: 3.3
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

| 序号 | 关节名称                       | 具体描述        |
|----|----------------------------|-------------|
| 0  | left_hip_pitch_joint       | 左髋 pitch 关节 |
| 1  | left_hip_roll_joint        | 左髋 roll 关节  |
| 2  | left_hip_yaw_joint         | 左髋 yaw 关节   |
| 3  | left_knee_pitch_joint      | 左膝 pitch 关节 |
| 4  | left_ankle_roll_joint      | 左踝 roll 关节  |
| 5  | left_ankle_pitch_joint     | 左踝 pitch 关节 |
| 6  | right_hip_pitch_joint      | 右髋 pitch 关节 |
| 7  | right_hip_roll_joint       | 右髋 roll 关节  |
| 8  | right_hip_yaw_joint        | 右髋 yaw 关节   |
| 9  | right_knee_pitch_joint     | 右膝 pitch 关节 |
| 10 | right_ankle_roll_joint     | 右踝 roll 关节  |
| 11 | right_ankle_pitch_joint    | 右踝 pitch 关节 |
| 12 | waist_yaw_joint            | 腰部 yaw 关节   |
| 13 | left_shoulder_pitch_joint  | 左肩 pitch 关节 |
| 14 | left_shoulder_roll_joint   | 左肩 roll 关节  |
| 15 | left_shoulder_yaw_joint    | 左肩 yaw 关节   |
| 16 | left_elbow_pitch_joint     | 左肘 pitch 关节 |
| 17 | left_wrist_yaw_joint       | 左腕 yaw 关节   |
| 18 | right_shoulder_pitch_joint | 右肩 pitch 关节 |
| 19 | right_shoulder_roll_joint  | 右肩 roll 关节  |
| 20 | right_shoulder_yaw_joint   | 右肩 yaw 关节   |
| 21 | right_elbow_pitch_joint    | 右肘 pitch 关节 |
| 22 | right_wrist_yaw_joint      | 右腕 yaw 关节   |

