---
layout: default
title: 命令行工具
nav_order: 3.7
parent: 参考指南
has_toc: true
---

# 命令行工具

在完成 Fourier-GRX-N1 SDK 的安装后，用户可以使用命令行工具 `fourier-grx` 来管理和控制机器人。

`fourier-grx` 是 Fourier-GRX-N1 SDK 的命令行工具，用于安装、配置和控制 Fourier N1 机器人。

通过在终端运行 `fourier-grx` 命令，会在终端中显示可用的子命令列表和帮助信息。

目前，支持的命令行工具子命令包括：

| 子命令               | 说明                                             |
|-------------------|------------------------------------------------|
| `version`         | 显示当前 Fourier-GRX-N1 SDK 的版本信息                  |
| `install`         | 安装 Fourier-GRX-N1 SDK 运行环境和依赖库                 |
| `uninstall`       | 卸载 Fourier-GRX-N1 SDK 运行环境和依赖库                 |
| `config`          | 配置 Fourier-GRX-N1 SDK 的启动参数                    |
| `list`            | 列出 Fourier-GRX-N1 SDK 的启动参数                    |
| `start`           | 启动 Fourier-GRX-N1 SDK 运行环境和机器人                 |
| `stop`            | 停止 Fourier-GRX-N1 SDK 运行环境和机器人 （强制杀死相关进程）      |
| `enable_service`  | 启用 Fourier-GRX-N1 SDK 的服务功能，电脑重启后会自动启动机器人控制程序  |
| `disable_service` | 禁用 Fourier-GRX-N1 SDK 的服务功能，电脑重启后不会自动启动机器人控制程序 |
| `background`      | 将 Fourier-GRX-N1 SDK 运行环境和机器人程序放到后台运行          |
| `setup_conda`     | 创建用于 Fourier-GRX-N1 SDK 的 Conda 环境，并安装所需的依赖库   |