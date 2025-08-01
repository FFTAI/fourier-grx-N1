---
layout: default
title: 示例代码
nav_order: 2
has_toc: true
---

# 示例代码

* TOC
{:toc}

本文档提供了丰富的示例代码，帮助您快速掌握 Fourier-GRX-N1 SDK 的使用方法。

- N1 系列机器人的示例代码位于 [Github Wiki-GRx-Deploy](https://github.com/FFTAI/Wiki-GRx-Deploy.git) 的 `FourierN1` 分支中。

示例代码包含用户接口（User API）、开发者接口（Developer API）、以及 PubSub 接口的示例，适用于不同层次的开发需求。

## 二次开发环境配置

`fourier-grx` 工具提供了 `fourier-grx setup_conda` 命令用于一键配置 conda 开发环境用于机器人二次开发。安装环境时，请确保机器网络连接正常。

```bash
# 在机器人主控电脑上：
fourier-grx setup_conda

# 程序运行完成后，会搭建出一个名为 `fourier-grx` 的 conda 环境，可以通过以下命令激活该环境
conda activate fourier-grx

# 如果希望自主搭建开发环境，可以在 $HOME/fourier-grx/whl 中找到依赖库文件进行手动安装。
```

## 示例程序下载

可以通过 `git` 工具同步机器人的二次开发接口示例程序，同步命令为：

```bash
# 在机器人主控电脑 $HOME 目录下执行
git clone https://github.com/FFTAI/Wiki-GRx-Deploy.git --branch=FourierN1
cd $HOME/Wiki-GRx-Deploy
```

建议同步到 `$HOME` 目录下，同步完成后，可以通过 `cd $HOME/Wiki-GRx-Deploy` 进入该目录查看。

运行具体的示例程序，请参考各个接口的示例代码说明。
