---
layout: default
title: 固件更新
nav_order: 8
toc: true          # 启用目录
toc_min_header: 2  # 最小显示标题层级（如 H2）
toc_max_header: 3  # 最大显示标题层级（如 H3）
---

# 固件更新

## 版本发布

| 发布日期       | 版本号    | 资源链接                                                                                         | 更新内容                                                                                                                                                                                                               |
|------------|--------|----------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 2025-05-19 | 2.3.24 | [下载](https://fourier-grx-1302548221.cos.ap-shanghai.myqcloud.com/grx/fourier-grx-2.3.24.deb) | 1. 优化使能、失能任务，防止失能后再使能出现冲击动作                                                                                                                                                                                        |
| 2025-05-15 | 2.3.22 | [下载](https://fourier-grx-1302548221.cos.ap-shanghai.myqcloud.com/grx/fourier-grx-2.3.22.deb) | 1. 修复机器人切换任务时，模块任务状态仍然保留的问题 <br/> 2. 修复 N1 机器人从其他任务切换到模仿学习任务，启动时抽动问题 <br/> 3. 修复 N1 机器人模仿学习下蹲动作开始和结束时站不稳的问题 <br/> 4. 优化安装过程中的提示信息 <br/> 5. 添加 SDK 控制接口方法 reboot servo （重启执行器） <br/> 6. 优化 SDK 开发中线程之间数据争抢导致的数据错误问题 |
| 2025-04-30 | 2.3.19 | [下载](https://fourier-grx-1302548221.cos.ap-shanghai.myqcloud.com/grx/fourier-grx-2.3.19.deb) | 1. 添加 fourier-grx setup_conda 自动配置开发环境 <br/> 2. 添加内置爬起运动算法 <br/> 3. 添加内置单脚站立运动算法 <br/> 4. 优化 N1 机器人任务清单 <br/> 5. 修复已知问题                                                                                            |
| 2025-03-26 | 2.3.2  | [下载](https://fourier-grx-1302548221.cos.ap-shanghai.myqcloud.com/grx/fourier-grx-2.3.2.deb)  | 1. 添加 fourier-grx enable_service 配置开启程序自启动功能 <br/> 2. 添加 fourier-grx disable_service 配置关闭程序自启动功能 <br/> 3. 优化打印 log 信息。 <br/> 4. 优化配置文件读取逻辑：配置文件错误，直接退出程序                                                           |
| 2025-03-24 | 2.3.1  | [下载](https://fourier-grx-1302548221.cos.ap-shanghai.myqcloud.com/grx/fourier-grx-2.3.1.deb)  | 1. N1 系列机器人的固件更新。                                                                                                                                                                                                  |

## 安装流程

固件安装流程请参考 [固件安装和更新](/fourier-grx-N1/docs/usage#固件安装和更新)。