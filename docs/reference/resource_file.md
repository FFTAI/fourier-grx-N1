---
layout: default
title: 资源文件
nav_order: 3.5
parent: 参考指南
has_toc: true
---

# 资源文件

* TOC
  {:toc}

资源文件是 Fourier-GRX SDK 中用于存储和管理机器人的资源数据的文件，通常包括模型文件（机器人模型、神经网络模型）、参考数据、用户名密码信息等。

## 资源文件的路径

资源文件通常存储在 `~/fourier-grx/resource/` 目录下。不同机器人的资源文件存储在不同的子目录中。

例如，`FourierN1` 机器人的资源文件存储在 `~/fourier-grx/resource/n1/` 目录下。

## Zenoh 资源文件

Zenoh 资源文件是 Fourier-GRX SDK 中用于存储和管理机器人的 Zenoh 相关资源数据的文件，通常包括 Zenoh 的配置文件、用户名密码、授权信息等。

Zenoh 资源文件通常存储在 `~/fourier-grx/resource/zenoh/` 目录下。

目前，zenoh 资源文件的内容包括：
- `user.yaml`: 存储 Zenoh 用户名和密码信息（即本地 Zenoh 用户名和密码）。
- `credentials.yaml`: 存储 Zenoh 的授权信息（即接受的 Zenoh 用户名和密码）。

通常情况下，`user.yaml` 和 `credentials.yaml` 中的用户名和密码是相同的，以确保程序本地正常通信。

具体的内容可以参考 https://zenoh.io/docs/manual/user-password/