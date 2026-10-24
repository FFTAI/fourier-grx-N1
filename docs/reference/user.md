---
layout: default
title: User 接口
nav_order: 4.1
parent: 参考指南
has_toc: true
---

# 参考指南 User 接口

* TOC
{:toc}

> ℹ️ **说明**：
>
> 使用 Fourier-GRX-N1 SDK User API 前，请将 `fourier-grx` 配置为 **开发者模式**。
> 关于运行模式的配置，请参见 [运行模式](/fourier-grx-N1/docs/reference/run_type)。

> ⚠️ **当前 N1 版本说明**：
>
> User API 需要同时启用 `dynalink`、`communication` 和 `sync` 三个配置段；随包 N1 配置默认未启用 `sync`，如需使用请手动在配置文件中设置 `sync.enable: true`。
>
> 部分状态字段（控制模式、加速度、末端状态、机器人/任务标志等）在当前版本中仍为预留字段，可能不会实时更新；`joint_output_current` 当前与目标力矩同值。请勿将这些字段用于安全判断或闭环控制。

Fourier-GRX User 接口使用 [Zenoh](https://zenoh.io/) 协议与机器人通信，
适用于高层应用开发。

User 接口主要分为以下5类：

- `comm`：通信相关信息
- `core`：核心库相关信息
- `robot`：机器人状态信息
- `task`：任务相关信息
- `grx`：fourier-grx 库相关信息，通用机器人相关内容（主要是人形）

对应的 zenoh 接口 topic 的 key 格式为：

- key 可以是 ["comm", "core", "robot", "task", "grx"]
- `fourier-grx/dynalink_interface/{key}/server` ：机器人主控程序发出的状态信息 **[机器人作为服务器]**
- `fourier-grx/dynalink_interface/{key}/client` ：机器人主控程序接收的指令信息 **[用户机作为客户端]**

---

## 4.0.0 以上版本

### 状态信息

#### comm/server 接口协议 (状态信息)

key 说明列表：

| key               | 说明   | 数据类型 | 具体描述      |
|-------------------|------|------|-----------|
| `flag_heart_beat` | 心跳标志 | bool | 1: 机器人已启动 |

#### robot/server 接口协议 (状态信息)

key 说明列表：

| key                                   | 说明         | 数据类型                              | 单位    | 具体描述                                    |
|---------------------------------------|------------|-----------------------------------|-------|-----------------------------------------|
| `sensor_imus_quat_value`              | 传感器 IMU 数据 | array(float, float, float, float) |       | 传感器 IMU 数据，四元数表示姿态，顺序为 x, y, z, w       |
| `sensor_imus_euler_angle_value`       | 传感器 IMU 数据 | array(float, float, float)        | rad   | 传感器 IMU 数据，欧拉角表示姿态，顺序为 roll, pitch, yaw |
| `sensor_imus_angular_velocity_value`  | 传感器 IMU 数据 | array(float, float, float)        | rad/s | 传感器 IMU 数据，角速度，顺序为 x, y, z              |
| `sensor_imus_linear_acceleration_value` | 传感器 IMU 数据 | array(float, float, float)      | m/s^2 | 传感器 IMU 数据，线加速度，顺序为 x, y, z             |

| key                              | 说明   | 数据类型         | 单位      | 具体描述    |
|----------------------------------|------|--------------|---------|---------|
| `flag_actuator_installed`        | 电机安装标志 | array(bool)  |         | 0: 未安装，1: 已安装 |
| `flag_actuator_accessible`       | 电机通信标志 | array(bool)  |         | 0: 不可通信，1: 可通信 |
| `flag_actuator_enables`          | 电机使能标志 | array(bool)  |         | 0: 失能，1: 使能 |
| `flag_actuator_error`            | 电机错误标志 | array(bool)  |         | 0: 无错误，1: 有错误 |
| `actuator_measured_control_mode` | 电机控制模式 | array(int)   |         | 电机测量控制模式 |
| `actuator_measured_position`     | 电机位置 | array(float) | deg     | 电机测量位置 |
| `actuator_measured_velocity`     | 电机速度 | array(float) | deg/s   | 电机测量速度 |
| `actuator_measured_acceleration` | 电机加速度 | array(float) | deg/s^2 | 电机测量加速度 |
| `actuator_measured_effort`       | 电机力矩 | array(float) | Nm      | 电机测量力矩 |
| `actuator_measured_current`      | 电机电流 | array(float) | A       | 电机测量电流 |
| `actuator_output_control_mode`   | 电机控制模式 | array(int)   |         | 电机指令控制模式 |
| `actuator_output_position`       | 电机位置 | array(float) | deg     | 电机指令位置 |
| `actuator_output_velocity`       | 电机速度 | array(float) | deg/s   | 电机指令速度 |
| `actuator_output_acceleration`   | 电机加速度 | array(float) | deg/s^2 | 电机指令加速度 |
| `actuator_output_effort`         | 电机力矩 | array(float) | Nm      | 电机指令力矩 |
| `actuator_output_current`        | 电机电流 | array(float) | A       | 电机指令电流 |

| key                           | 说明   | 数据类型         | 单位      | 具体描述    |
|-------------------------------|------|--------------|---------|---------|
| `joint_mode_of_operation`     | 关节操作模式 | array(int)   |         | 关节操作模式 |
| `joint_control_word`          | 关节控制字 | array(int)   |         | 关节控制字 |
| `joint_status_word`           | 关节状态字 | array(int)   |         | 关节状态字 |
| `joint_application_position`  | 关节位置 | array(float) | rad     | 关节应用位置 |
| `joint_application_velocity`  | 关节速度 | array(float) | rad/s   | 关节应用速度 |
| `joint_application_effort`    | 关节力矩 | array(float) | Nm      | 关节应用力矩 |
| `joint_measured_control_mode` | 关节控制模式 | array(int)   |         | 关节测量控制模式 |
| `joint_measured_position`     | 关节位置 | array(float) | rad     | 关节测量位置 |
| `joint_measured_velocity`     | 关节速度 | array(float) | rad/s   | 关节测量速度 |
| `joint_measured_acceleration` | 关节加速度 | array(float) | rad/s^2 | 关节测量加速度 |
| `joint_measured_effort`       | 关节力矩 | array(float) | Nm      | 关节测量力矩 |
| `joint_measured_current`      | 关节电流 | array(float) | A       | 关节测量电流 |
| `joint_output_control_mode`   | 关节控制模式 | array(int)   |         | 关节指令控制模式 |
| `joint_output_position`       | 关节位置 | array(float) | rad     | 关节指令位置 |
| `joint_output_velocity`       | 关节速度 | array(float) | rad/s   | 关节指令速度 |
| `joint_output_acceleration`   | 关节加速度 | array(float) | rad/s^2 | 关节指令加速度 |
| `joint_output_effort`         | 关节力矩 | array(float) | Nm      | 关节指令力矩 |
| `joint_output_current`        | 关节电流 | array(float) | A       | 关节指令电流 |

| key                                | 说明   | 数据类型         | 单位      | 具体描述   |
|------------------------------------|------|--------------|---------|--------|
| `end_effector_measured_position`   | 末端位姿 | array(float) |         | 末端测量位姿 |
| `end_effector_measured_velocity`   | 末端速度 | array(float) |         | 末端测量速度 |
| `end_effector_measured_acceleration` | 末端加速度 | array(float) |       | 末端测量加速度 |
| `end_effector_measured_effort`     | 末端力矩 | array(float) | Nm      | 末端测量力矩 |

| key                          | 说明   | 数据类型  | 单位 | 具体描述    |
|------------------------------|------|---------|----|---------|
| `flag_robot_self_check`      | 机器人自检标志 | bool |    | 0: 未自检，1: 已自检 |
| `flag_robot_calibration`     | 机器人校准标志 | bool |    | 0: 未校准，1: 已校准 |
| `flag_robot_servo_on`        | 机器人使能标志 | bool |    | 0: 失能，1: 使能 |
| `flag_robot_emergent_stop`   | 机器人急停标志 | bool |    | 0: 未急停，1: 急停 |
| `flag_robot_fault`           | 机器人故障标志 | bool |    | 0: 无故障，1: 有故障 |
| `flag_robot_error`           | 机器人错误标志 | bool |    | 0: 无错误，1: 有错误 |
| `flag_robot_pinched`         | 机器人挤压标志 | bool |    | 0: 未挤压，1: 挤压 |
| `flag_robot_over_load`       | 机器人过载标志 | bool |    | 0: 未过载，1: 过载 |
| `flag_robot_torque_protection` | 机器人力矩保护标志 | bool |  | 0: 未保护，1: 保护 |
| `robot_name`                 | 机器人名称 | string |    | 机器人名称 |
| `robot_work_space`           | 机器人工作空间 | int  |    | 机器人当前工作空间 |

#### task/server 接口协议 (状态信息)

key 说明列表：

| key                        | 说明       | 数据类型 | 具体描述                                                                                             |
|----------------------------|----------|------|--------------------------------------------------------------------------------------------------|
| `flag_task_start`          | 任务是否已开始 | bool | 0: 任务未开始，1: 任务已开始                                                                            |
| `flag_task_running`        | 任务是否运行中 | bool | 0: 任务未运行，1: 任务运行中                                                                            |
| `flag_task_finish`         | 任务是否已结束 | bool | 0: 任务未结束，1: 任务已结束                                                                            |
| `flag_task_pause`          | 任务是否已暂停 | bool | 0: 任务未暂停，1: 任务已暂停                                                                            |
| `robot_task_state`         | 机器人任务状态  | int  | 当前机器人任务状态，如果设置了 task/client 中的 `flag_task_command_update` 为 True，会更新此值，更新值为 `robot_task_command` |
| `robot_task_state_data`    | 机器人任务状态数据 | dict | 机器人任务状态数据，不同任务数值含义不同                                                                       |
| `flag_component_start`     | 模块是否已开始 | bool | 0: 模块未开始，1: 模块已开始                                                                            |
| `flag_component_in_process` | 模块是否进行中 | bool | 0: 模块未进行，1: 模块进行中                                                                            |
| `flag_component_finish`    | 模块是否已结束 | bool | 0: 模块未结束，1: 模块已结束                                                                            |
| `robot_component_state`    | 机器人模块状态  | int  | 当前机器人模块状态                                                                                      |
| `robot_component_state_data` | 机器人模块状态数据 | dict | 机器人模块状态数据，不同模块数值含义不同                                                                      |

#### grx/server 接口协议 (状态信息)

key 说明列表：

| key                    | 说明     | 数据类型        | 具体描述                    |
|------------------------|--------|-------------|-------------------------|
| `fourier_core_version` | 核心库版本  | string      | 核心库版本号                  |
| `fourier_grx_version`  | GRX库版本 | string      | GRX库版本号                 |
| `robot_error_codes`    | 机器人错误码 | array(int)  | 机器人错误码列表，空列表: 无错误，其他: 具体错误码 |

---

### 指令信息

#### comm/client 接口协议 (指令信息)

key 说明列表：

| key | 说明 | 数据类型 | 具体描述 |
|-----|----|------|------|
|     |    |      |      |

#### robot/client 接口协议 (指令信息)

key 说明列表：

| key | 说明 | 数据类型 | 具体描述 |
|-----|----|------|------|
|     |    |      |      |

#### task/client 接口协议 (指令信息)

任务指令发送接口如下，发送指令时要求按照以下流程执行：

- 任务指令发送：
    1. 发送 robot_task_command 和 robot_task_command_data，任务相关信息
    2. 发送 flag_task_command_update 指令更新标志位，确认更新任务指令
- 模块指令发送：
    1. 发送 robot_component_command 和 robot_component_command_data，模块相关信息
    2. 发送 flag_component_command_update 指令更新标志位，确认更新模块指令

key 说明列表：

| key                             | 说明         | 数据类型 | 具体描述            |
|---------------------------------|------------|------|-----------------|
| `flag_task_command_update`      | 任务指令请求更新标志 | bool | 0: 不更新，1: 更新    |
| `robot_task_command`            | 任务指令       | int  | 机器人 `任务值 (TID)` |
| `robot_task_command_data`       | 任务指令数据     | dict | 机器人任务指令数据       |
| `flag_component_command_update` | 模块指令请求更新标志 | bool | 0: 不更新，1: 更新    |
| `robot_component_command`       | 模块指令       | int  | 机器人 `模块值 (MID)` |
| `robot_component_command_data`  | 模块指令数据     | dict | 机器人模块指令数据       |

#### grx/client 接口协议 (指令信息)

key 说明列表：

- 为规范用户的高层控制指令传入，对输入参数进行了封装和抽象。主要定义了“虚拟外设”的概念来进行机器人的控制。
    - 虚拟摇杆 (virtual joystick)
    - 虚拟键盘 (virtual keyboard)
    - 虚拟用户 (virtual user) 用于存储用户信息
    - 虚拟面板 (virtual panel) 用于更通用的信息输入
- 在需要使用到对应虚拟设备时，需要在主程序启动时调用的 `config_xxx.yaml` 文件中，设置虚拟外设为 `True`，如：
    - `peripheral/use_virtual_joystick`
    - `peripheral/use_virtual_keyboard`
    - `peripheral/use_virtual_user`
    - `peripheral/use_virtual_panel`
- 一个任务可能允许使用多个虚拟外设进行控制，但是由于输入数据的唯一性，外设之间输入数据会互相覆盖，因此，建议用户参看具体任务中关于外设数据优先级的说明。
    - 优先级高的外设会覆盖优先级低的外设输入数据。

| key                             | 说明           | 数据类型                | 具体描述                 |
|---------------------------------|--------------|---------------------|----------------------|
| `virtual_joystick_button_up`    | 虚拟手柄上按钮状态    | int                 | 0: 未按下，1: 按下         |
| `virtual_joystick_button_down`  | 虚拟手柄下按钮状态    | int                 | 0: 未按下，1: 按下         |
| `virtual_joystick_button_left`  | 虚拟手柄左按钮状态    | int                 | 0: 未按下，1: 按下         |
| `virtual_joystick_button_right` | 虚拟手柄右按钮状态    | int                 | 0: 未按下，1: 按下         |
| `virtual_joystick_button_l1`    | 虚拟手柄 L1 按钮状态 | int                 | 0: 未按下，1: 按下         |
| `virtual_joystick_button_l2`    | 虚拟手柄 L2 按钮状态 | int                 | 0: 未按下，1: 按下         |
| `virtual_joystick_button_r1`    | 虚拟手柄 R1 按钮状态 | int                 | 0: 未按下，1: 按下         |
| `virtual_joystick_button_r2`    | 虚拟手柄 R2 按钮状态 | int                 | 0: 未按下，1: 按下         |
| `virtual_joystick_axis_left`    | 虚拟手柄左摇杆状态    | array(float, float) | 摇杆状态值范围为 [-1.0, 1.0] |
| `virtual_joystick_axis_right`   | 虚拟手柄右摇杆状态    | array(float, float) | 摇杆状态值范围为 [-1.0, 1.0] |

| key                          | 说明           | 数据类型 | 具体描述         |
|------------------------------|--------------|------|--------------|
| `virtual_keyboard_key_up`    | 虚拟键盘上键状态     | int  | 0: 未按下，1: 按下 |
| `virtual_keyboard_key_down`  | 虚拟键盘下键状态     | int  | 0: 未按下，1: 按下 |
| `virtual_keyboard_key_left`  | 虚拟键盘左键状态     | int  | 0: 未按下，1: 按下 |
| `virtual_keyboard_key_right` | 虚拟键盘右键状态     | int  | 0: 未按下，1: 按下 |
| `virtual_keyboard_key_enter` | 虚拟键盘回车键状态    | int  | 0: 未按下，1: 按下 |
| `virtual_keyboard_key_esc`   | 虚拟键盘 ESC 键状态 | int  | 0: 未按下，1: 按下 |
| `virtual_keyboard_key_f1`    | 虚拟键盘 F1 键状态  | int  | 0: 未按下，1: 按下 |
| `virtual_keyboard_key_f2`    | 虚拟键盘 F2 键状态  | int  | 0: 未按下，1: 按下 |
| `virtual_keyboard_key_f3`    | 虚拟键盘 F3 键状态  | int  | 0: 未按下，1: 按下 |
| `virtual_keyboard_key_f4`    | 虚拟键盘 F4 键状态  | int  | 0: 未按下，1: 按下 |
| `virtual_keyboard_key_q`     | 虚拟键盘 Q 键状态   | int  | 0: 未按下，1: 按下 |
| `virtual_keyboard_key_w`     | 虚拟键盘 W 键状态   | int  | 0: 未按下，1: 按下 |
| `virtual_keyboard_key_e`     | 虚拟键盘 E 键状态   | int  | 0: 未按下，1: 按下 |
| `virtual_keyboard_key_a`     | 虚拟键盘 A 键状态   | int  | 0: 未按下，1: 按下 |
| `virtual_keyboard_key_s`     | 虚拟键盘 S 键状态   | int  | 0: 未按下，1: 按下 |
| `virtual_keyboard_key_d`     | 虚拟键盘 D 键状态   | int  | 0: 未按下，1: 按下 |

| key                                   | 说明    | 数据类型  | 具体描述          |
|---------------------------------------|-------|-------|---------------|
| `virtual_user_upper_leg_length_left`  | 左上肢长度 | float | 单位为米，默认值为 0.5 |
| `virtual_user_upper_leg_length_right` | 右上肢长度 | float | 单位为米，默认值为 0.5 |
| `virtual_user_lower_leg_length_left`  | 左下肢长度 | float | 单位为米，默认值为 0.5 |
| `virtual_user_lower_leg_length_right` | 右下肢长度 | float | 单位为米，默认值为 0.5 |
| `virtual_user_upper_arm_length_left`  | 左上臂长度 | float | 单位为米，默认值为 0.5 |
| `virtual_user_upper_arm_length_right` | 右上臂长度 | float | 单位为米，默认值为 0.5 |
| `virtual_user_lower_arm_length_left`  | 左下臂长度 | float | 单位为米，默认值为 0.5 |
| `virtual_user_lower_arm_length_right` | 右下臂长度 | float | 单位为米，默认值为 0.5 |
| `virtual_user_knee_restriction_left`  | 左膝关节限制 | float | 针对康复患者的左膝关节限制参数 |
| `virtual_user_knee_restriction_right` | 右膝关节限制 | float | 针对康复患者的右膝关节限制参数 |

| key                              | 说明       | 数据类型  | 具体描述                   |
|----------------------------------|----------|-------|------------------------|
| `virtual_panel_command_param_1`  | 面板命令参数1  | float | 面板命令参数1，具体含义由任务决定      |
| `virtual_panel_command_param_2`  | 面板命令参数2  | float | 面板命令参数2，具体含义由任务决定      |
| `virtual_panel_command_param_3`  | 面板命令参数3  | float | 面板命令参数3，具体含义由任务决定      |
| `virtual_panel_command_param_4`  | 面板命令参数4  | float | 面板命令参数4，具体含义由任务决定      |
| `virtual_panel_command_param_5`  | 面板命令参数5  | float | 面板命令参数5，具体含义由任务决定      |
| `virtual_panel_command_param_6`  | 面板命令参数6  | float | 面板命令参数6，具体含义由任务决定      |
| `virtual_panel_command_param_7`  | 面板命令参数7  | float | 面板命令参数7，具体含义由任务决定      |
| `virtual_panel_command_param_8`  | 面板命令参数8  | float | 面板命令参数8，具体含义由任务决定      |
| `virtual_panel_command_param_9`  | 面板命令参数9  | float | 面板命令参数9，具体含义由任务决定      |
| `virtual_panel_command_switch_1` | 面板命令开关1  | bool  | 0: 不开，1: 开，具体含义由任务决定   |
| `virtual_panel_command_switch_2` | 面板命令开关2  | bool  | 0: 不开，1: 开，具体含义由任务决定   |
| `virtual_panel_command_switch_3` | 面板命令开关3  | bool  | 0: 不开，1: 开，具体含义由任务决定   |
| `virtual_panel_command_switch_4` | 面板命令开关4  | bool  | 0: 不开，1: 开，具体含义由任务决定   |
| `virtual_panel_command_switch_5` | 面板命令开关5  | bool  | 0: 不开，1: 开，具体含义由任务决定   |
| `virtual_panel_command_picker_1` | 面板命令选择器1 | int   | 面板命令选择器1，具体含义由任务决定     |
| `virtual_panel_command_picker_2` | 面板命令选择器2 | int   | 面板命令选择器2，具体含义由任务决定     |
| `virtual_panel_command_picker_3` | 面板命令选择器3 | int   | 面板命令选择器3，具体含义由任务决定     |
| `virtual_panel_command_start`    | 面板命令开始标志 | bool  | 0: 不开始，1: 开始，具体含义由任务决定 |
| `virtual_panel_command_stop`     | 面板命令停止标志 | bool  | 0: 不停止，1: 停止，具体含义由任务决定 |
| `virtual_panel_command_pause`    | 面板命令暂停标志 | bool  | 0: 不暂停，1: 暂停，具体含义由任务决定 |
