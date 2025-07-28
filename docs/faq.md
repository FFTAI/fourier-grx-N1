---
layout: default
title: 常见问题
nav_order: 8
has_toc: true
---

# 常见问题解答（FAQ）

* TOC
{:toc}

本文档收集了使用 Fourier-GRX-N1 SDK 时常见的问题和解决方案。如果您遇到的问题未在此列出，请联系技术支持。

## 硬件问题

### 电池充电充不进去

**问题描述**：机器人插线充电充不进去。充了很久，一上电机器仍然自动断电了。

**解决方案**：

1. 检查充电线是否正确稳定连接到充电口
2. 检查充电器是否正常工作（显示红灯或蓝灯为充电状态，绿灯充满电）
3. 检查电池开关按钮是否按下，**只有按下状态**，电池才会充电。否则是电源线只给机器人供电，不给电池充电。
4. 检查是否保险丝烧断。
    - 保险丝在电池盒内，打开电池盒后可以看到保险丝的状态
    - 如果烧断了，需更换保险丝

## 安装问题

### 安装过程中断

**问题描述**：程序安装过程中由于意外或操作失误导致的安装程序退出。

**解决方案**：

1. 重新运行安装程序即可，旧有安装内容会被清理掉

### 机型配置失败

**问题描述**：运行安装程序过程中请求输入数字以配置机型，但输入后报配置错误，退出安装。

**解决方案**：

1. 检查输入的 **数字** 是否正确，不是输入具体的选项名称
2. 重启安装程序后重新配置

## 初始化问题

### 配置文件错误

**问题描述**：机器人初始化失败，提示配置文件错误

**解决方案**：

1. 确认机器人型号是否配置正确，参见 [固件安装和更新](/fourier-grx-N1/docs/usage#固件安装和更新)
2. 重启机器人后重试

### IMU 初始化失败

**问题描述**：机器人 initialize 失败，提示找不到 IMU 设备

![IMU初始化错误](/fourier-grx-N1/assets/images/initialize_imu_error.png)

**解决方案**：

1. 检查 IMU 连接线是否正确连接到机器人主控板
2. 确认 IMU 设备是否损坏
3. 重启机器人后重试

### 执行器自检失败

**问题描述**：机器人 self-check 失败，提示无法访问指定 IP 的执行器

![自检错误](/fourier-grx-N1/assets/images/self_check_error.png)

**解决方案**：

1. 检查执行器电源状态（应显示紫色呼吸灯）
2. 确认有线网络连接和静态 IP 配置
3. 检查线路连接完整性

### IMU 数据异常

**问题描述**：机器人初始化成功，但 IMU 数据异常，一直报错

![IMU 数据错误 1.png](/fourier-grx-N1/assets/images/imu_data_error_quat_zero.png)

![IMU 数据错误 2.png](/fourier-grx-N1/assets/images/imu_data_error_error_frame.png)

**解决方案**：

1. 检查 IMU 连接线是否正确连接到机器人主控板
2. 检查是否 fourier-grx 程序重复启动 （重复启动，会竞争设备权属，导致数据出错）
    - 终端输入 `ps -ef | grep fourier-grx` 查看是否有多个 fourier-grx 进程
    - 杀死已启动的 fourier-grx 程序：`sudo killall fourier-grx`
    - 重新启动 fourier-grx 程序
3. 检查是否配置了 fourier-grx 开机自启动
    - 终端输入 `crontab -e`，查看是否配置了 fourier-grx 开机自启动
    - 如果配置了，可终端运行 `fourier-grx disable_service` 关闭开机自启动
    - 重启机器人控制电脑 💻

## 网络配置

### 外网访问配置

**问题**：如何配置机器人访问外网？

**解决方案**：

1. 通过有线网口连接外部网络

2. 配置步骤：

```bash
# 1. 切换到动态IP（访问外网时）
sudo nmcli connection modify "有线连接" ipv4.method auto
   
# 2. 切换回静态IP（使用机器人时）
sudo nmcli connection modify "有线连接" ipv4.method manual \
    ipv4.addresses 192.168.137.220/24
   
# 3. 重启网络服务
sudo systemctl restart NetworkManager
```

### WiFi 热点配置

**问题**：如何关闭 WiFi 热点自启动？

**解决方案**：

```bash
# 临时关闭
sudo systemctl stop rocs-wifi

# 永久关闭
sudo systemctl disable rocs-wifi

# 重启生效
sudo reboot
```

## 性能相关

### 控制频率说明

- **用户接口（User API）**：
    - 控制频率：50Hz
    - 状态输出：50Hz
    - 指令接收：50Hz

- **开发者接口（Developer API）**：
    - 数据更新频率：可配置，默认 400Hz（最高可配置为 500Hz）
    - 算法运行频率：可配置，建议不超过数据更新频率

### 超时警告处理

**问题描述**：程序报 Timeout 警告

**解决方案**：

1. 关闭 IPv6（主控电脑和局域网内其他设备）
2. 检查执行器连接线是否松动
3. 监控网络延迟和丢包情况

### 手柄休眠问题

**问题描述**：手柄不操作一段时间会出现休眠，休眠后无法再重新连上机器人进行控制。

**解决方案**：

1. 尝试使用判断休眠等待时间更长，或者是可以配置无休眠的手柄
    - 例如：Gamesir 的 G8+ Pro 手柄
    - 例如：北通星闪手柄
2. 重连手柄后重启机器人控制程序

## 开发环境问题

### 用户接口通信问题

**问题**：user 测试程序无法正常通信

**解决方案**：

1. 确认网络配置：
    - Zenoh 优先使用有线端口
    - 避免同时连接有线和无线网络
2. 检查网络连接状态
3. 确认 SDK 版本兼容性

### 依赖问题

**问题**：ImportError: GLIBC_2.33 not found

**解决方案**：

1. 安装必要的编译工具：

```bash
sudo apt update
sudo apt install build-essential
```

2. 系统要求：
    - 推荐：Ubuntu 22.04 LTS
    - 最低：支持 GLIBC 2.33 的 Linux 发行版

## 最佳实践

1. **开发环境准备**
    - 使用推荐的操作系统版本
    - 正确配置网络环境
    - 安装所有必要依赖

2. **网络配置**
    - 开发时使用静态IP
    - 需要外网时切换到动态IP
    - 定期检查网络连接状态

3. **故障排查**
    - 查看日志文件
    - 检查硬件连接
    - 监控系统资源

## 获取帮助

如果您遇到未在此文档中列出的问题：

1. 查看[开发文档](/fourier-grx-N1/docs/reference)
2. 检查[更新日志](/fourier-grx-N1/docs/changelog)
3. 联系公司技术支持：[xin.chen@fftai.com](mailto:xin.chen@fftai.com)
