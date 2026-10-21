---
layout: default
title: 启动配置文件
nav_order: 4.5
parent: 参考指南
has_toc: true
---

# 启动配置文件

* TOC
{:toc}

启动配置文件是 Fourier-GRX SDK 中用于配置机器人的启动参数的文件。用户可以通过修改该文件来实现对机器人的个性化配置。

## 启动配置文件的路径

启动 `fourier-grx` 时，需要通过 `--config` 参数指定启动配置文件的路径。
在使用 `fourier-grx start` 启动机器人时，具体使用的配置文件信息可以在 `~/fourier-grx/run.sh` 文件中找到。

默认情况下，`fourier-grx` 针对 `FourierN1` 机器人的启动配置文件位于 `~/fourier-grx/config/n1/` 目录下。

启动时使用哪个配置文件，取决于 `run.sh` 文件中的 `robot_type`, `robot_version` 和 `run_type` 字段。
这些字段的值既可以通过 `fourier-grx config` 命令行工具进行修改，也可以直接在 `run.sh` 文件中修改。

比如，如果用户希望启动自定义的机器人配置文件 `config_N1__custom.yaml`，可以在 `run.sh` 文件中将 `run_type` 字段修改为 `custom`：

```bash
robot_type="N1"
robot_version=""

run_type="custom"

# 此时，用 fourier-grx start 命令启动机器人时，将会使用 ~/fourier-grx/config/n1/config_N1__custom.yaml 作为配置文件。
```

## 启动配置文件的内容

启动配置文件是一个 YAML 格式的文件，包含了机器人的各种配置参数。以下是一些常见的配置项：

### 日志相关

| 配置项        | 说明                             | 数据类型    | 默认值    | 可选项                                          | 用户可修改 |
|------------|--------------------------------|---------|--------|----------------------------------------------|-------|
| log_config | 是否打印启动配置信息                     | boolean | false  | true, false                                  | 是     |
| log_level  | 日志级别                           | string  | "none" | "none", "trace", "debug", "warning", "error" | 是     |
| log_file   | 日志文件路径存储路径，仅针对日志中特定打印到日志文件中的内容 | string  | ""     | "filename", null                             | 是     |

### 控制系统相关

| 配置项              | 说明     | 数据类型    | 默认值     | 可选项                | 用户可修改 |
|------------------|--------|---------|---------|--------------------|-------|
| device_connected | 是否连接设备 | boolean | true    | true, false        | 是     |
| hardware_type    | 硬件类型   | string  | "X64"   |                    | 否     |
| hardware_layer   | 硬件层库    | string  | "fourier-core" |             | 否     |
| system           | 系统类型   | string  | "LINUX" |                    | 否     |
| mode             | 运行模式   | string  | "debug" | "debug", "release" | 否     |

### 调试相关

| 配置项                                           | 说明                         | 数据类型    | 默认值   | 可选项         | 用户可修改 |
|-----------------------------------------------|----------------------------|---------|-------|-------------|-------|
| debug: print_imu_state                        | 是否打印 IMU 原始状态信息            | boolean | false | true, false | 是     |
| debug: print_joint_application_position_state | 是否打印 关节 应用层位置状态信息        | boolean | false | true, false | 是     |
| debug: print_joint_application_velocity_state | 是否打印 关节 应用层速度状态信息        | boolean | false | true, false | 是     |
| debug: print_joint_application_effort_state   | 是否打印 关节 应用层力矩状态信息        | boolean | false | true, false | 是     |
| debug: print_joint_position_state             | 是否打印 关节 原始位置状态信息          | boolean | false | true, false | 是     |
| debug: print_joint_velocity_state             | 是否打印 关节 原始速度状态信息          | boolean | false | true, false | 是     |
| debug: print_joint_effort_state               | 是否打印 关节 原始力矩状态信息          | boolean | false | true, false | 是     |
| debug: print_joint_current_state              | 是否打印 关节 原始电流状态信息          | boolean | false | true, false | 是     |
| debug: print_joint_urdf_position_state        | 是否打印 关节 对应 URDF 文件位置状态信息  | boolean | false | true, false | 是     |
| debug: print_joint_urdf_velocity_state        | 是否打印 关节 对应 URDF 文件速度状态信息  | boolean | false | true, false | 是     |
| debug: print_joint_urdf_effort_state          | 是否打印 关节 对应 URDF 文件力矩状态信息  | boolean | false | true, false | 是     |
| debug: print_ioboard_state                    | 是否打印 IO 板状态信息             | boolean | false | true, false | 是     |
| debug: print_peripheral_virtual_joystick      | 是否打印 外设 虚拟摇杆状态信息           | boolean | false | true, false | 是     |
| debug: print_peripheral_virtual_keyboard      | 是否打印 外设 虚拟键盘状态信息           | boolean | false | true, false | 是     |
| debug: print_peripheral_virtual_mouse         | 是否打印 外设 虚拟鼠标状态信息           | boolean | false | true, false | 是     |
| debug: print_peripheral_virtual_panel         | 是否打印 外设 虚拟面板状态信息           | boolean | false | true, false | 是     |

### 子功能模块相关

| 配置项                   | 说明                                           | 数据类型    | 默认值   | 可选项         | 用户可修改           |
|-----------------------|----------------------------------------------|---------|-------|-------------|-----------------|
| dynalink: enable      | 是否启用 动态连接数据传输功能                              | boolean | false | true, false | 是               |
| communication: enable | 是否启用 通信循环功能（用于 dynalink 数据的发布等）              | boolean | false | true, false | 是               |
| communication: period | 通信循环周期，单位为秒；缺省为 1.0（1Hz）                     | float   | 1.0   | (>0)        | 是               |
| sync: enable          | 是否启用 数据同步功能，将 dynalink 数据通过 zenoh 同步给上位机（需要 dynalink:enable=true） | boolean | false | true, false | 是               |
| sync: period          | 数据同步周期，单位为秒；缺省为 1.0（1Hz）                     | float   | 1.0   | (>0)        | 是               |
| rerun: enable         | 是否启用 rerun 绘图功能（需要 dynalink:enable=true）     | boolean | false | true, false | 否 (当前版本未提供)    |
| streamlit: enable     | 是否启用 streamlit 绘图功能（需要 dynalink:enable=true） | boolean | false | true, false | 否 (暂未开放使用)      |
| teleoperation: enable | 是否启用 遥操作功能（需要 dynalink:enable=true）          | boolean | false | true, false | 否 (当前版本未提供) |
| pubsub: enable        | 是否启用 发布订阅功能，用于将机器人程序作为服务器，远程启动控制器作为客户端使用     | boolean | false | true, false | 是               |

### 资源文件相关

| 配置项            | 说明                                                        | 数据类型    | 默认值                            | 可选项                   | 用户可修改      |
|----------------|-----------------------------------------------------------|---------|--------------------------------|-----------------------|------------|
| resource: path | 资源文件路径                                                    | string  | "~/fourier-grx/resource/grmini1" | "path/to/resource"    | 是          |
| zenoh: path    | Zenoh 配置文件路径                                              | string  | "~/fourier-grx/resource/zenoh" | "path/to/zenoh"       | 否 (暂未开放使用) |
| zenoh: authentication | 是否启用 Zenoh 用户名/密码认证（凭据文件位于 zenoh: path 目录下） | boolean | false                          | true, false           | 是          |

> ℹ️ **说明**：
>
> 旧版本文档中提到的 `record: enable` / `record: path`（数据日志记录功能）在当前版本的代码中未提供。

### 外设相关

| 配置项                                   | 说明          | 数据类型    | 默认值      | 可选项                  | 用户可修改 |
|---------------------------------------|-------------|---------|----------|----------------------|-------|
| peripheral: use_joystick              | 是否使用手柄控制    | boolean | false    | true, false          | 是     |
| peripheral: joystick_type             | 手柄类型        | string  | "XBOX"   | "XBOX", "PS4", "PS5" | 是     |
| peripheral: use_keyboard              | 是否使用键盘控制    | boolean | false    | true, false          | 是     |
| peripheral: keyboard_type             | 键盘类型        | string  | "STANDARD" | "STANDARD"           | 是     |
| peripheral: use_virtual_joystick      | 是否使用虚拟手柄控制  | boolean | false    | true, false          | 是     |
| peripheral: use_virtual_keyboard      | 是否使用虚拟键盘控制  | boolean | false    | true, false          | 是     |
| peripheral: use_virtual_mouse         | 是否使用虚拟鼠标控制  | boolean | false    | true, false          | 是     |
| peripheral: use_virtual_user          | 是否使用虚拟用户控制   | boolean | false    | true, false          | 是     |
| peripheral: use_virtual_panel         | 是否使用虚拟面板控制  | boolean | false    | true, false          | 是     |

### 机器人相关

| 配置项                         | 说明        | 数据类型   | 默认值  | 可选项                                                 | 用户可修改 |
|-----------------------------|-----------|--------|------|-----------------------------------------------------|-------|
| robot: name                 | 机器人名称     | string | "N1" | "N1"                                                | 否     |
| robot: mechanism            | 机器人机械结构类型 | string | ""   | ""                                                  | 否     |
| robot: control_period       | 控制周期      | float  | 0.02 | (>0)，代码无强制下限；为获得稳定的控制效果，建议不低于 0.002 秒（500Hz） | 否     |

### 传感器相关

| 配置项                            | 说明                     | 数据类型           | 默认值                   | 可选项            | 用户可修改   |
|--------------------------------|------------------------|----------------|-----------------------|----------------|---------|
| sensor_usb_imu: usb            | IMU 传感器对应的 USB 设备路径    | array(string)  | ["/dev/ttyUSB0", ...] | "/path/to/USB" | 是（谨慎修改） |
| sensor_usb_imu: comm_enable    | 是否启用 USB IMU 传感器通信     | array(boolean) | [true, ...]           | true, false    | 是（谨慎修改） |
| sensor_usb_imu: comm_frequency | USB IMU 传感器通信频率，单位为 Hz | array(float)   | [500.0, ...]          |                | 是（谨慎修改） |

### 执行器相关

| 配置项                   | 说明        | 数据类型           | 默认值         | 可选项         | 用户可修改   |
|-----------------------|-----------|----------------|-------------|-------------|---------|
| actuator: comm_enable | 是否启用执行器通信 | array(boolean) | [true, ...] | true, false | 是（谨慎修改） |

| 配置项             | 说明       | 数据类型   | 默认值        | 可选项 | 用户可修改 |
|-----------------|----------|--------|------------|-----|-------|
| fi_fsa: version | FSA 库版本号 | string | "v2_async" |     | 否     |

