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

具体的内容可以参考 [https://zenoh.io/docs/manual/user-password/](https://zenoh.io/docs/manual/user-password/)

### 通过用户名和密码实现 Peer 模式下绑定

可以通过修改 `user.yaml` 和 `credentials.yaml` 文件中的用户名和密码，实现 Zenoh Peer 模式下的绑定。
可以实现在同一个局域网内，

- 控制主机1 <--> 控制指令/状态信息 <--> 机器人1
- 控制主机2 <--> 控制指令/状态信息 <--> 机器人2

具体步骤如下：

1. 修改配置文件，启用 zenoh 用户密码校验（修改启动时调用的 `config_xxx.json` 配置文件）:

    ```yaml
    # 机器人1 & 机器人2
    authentication: true  # 启用用户密码校验（默认是关闭校验功能的，局域网内各个节点可以直接通信）
    path: "~/fourier-grx/resource/zenoh"
    ```

2. 打开 `~/fourier-grx/resource/zenoh/user.yaml` 文件，修改其中的用户名和密码。例如：

    ```yaml
    # 机器人1
    user: "fourier-grx-1"
    password: "fourier-grx-1"
    ```
    ```yaml
    # 机器人2
    user: "fourier-grx-2"
    password: "fourier-grx-2"
    ```

3. 打开 `~/fourier-grx/resource/zenoh/credentials.txt` 文件，修改其中的用户名和密码，使其与 `user.yaml` 文件中的用户名和密码相同。例如：

    ```yaml
    # 机器人1
    # credentials.txt
    fourier-grx-1:fourier-grx-1
    ```
    ```yaml
    # 机器人2
    # credentials.txt
    fourier-grx-2:fourier-grx-2
    ```

4. 修改调用接口代码 `Wiki-GRx-Deploy` 中的 `credentials.txt` 内容，以及可能需要修改文件命名。例如：

    ```yaml
    # 控制主机1
    # credentials-1.txt
    fourier-grx-1:fourier-grx-1
    ```
    ```yaml
    # 控制主机2
    # credentials-2.txt
    fourier-grx-2:fourier-grx-2
    ```

5. 打开调用接口代码 `Wiki-GRx-Deploy` ，修改 Zenoh 的连接参数，指定用户名和密码。例如：

    ```python
    # 控制主机1
    import os, json
    import zenoh
   
    # 获取当前文件所在目录路径
    current_dir = os.path.dirname(os.path.abspath(__file__))
    credentials_path = os.path.join(current_dir, "credentials-1.txt")

    # 初始化 zenoh 配置 （旧版本）
    # zenoh_config = zenoh.Config()

    # 初始化 zenoh 配置 （新版本）
    zenoh_config = zenoh.Config.from_json5(
        json=json.dumps(
            {
                "mode": "peer",
                "transport": {
                    "auth": {
                        "usrpwd": {
                            "user": "fourier-grx-1",  # 修改为匹配当前通信环境的 username
                            "password": "fourier-grx-1",  # 修改为匹配当前通信环境的 password
                            "dictionary_file": credentials_path,  # 修改为匹配目标 fourier-grx 的 credentials.txt 路径
                        }
                    },
                },
            }
        )
    )

    # 创建 zenoh 会话
    zenoh_session: zenoh.Session = zenoh.open(zenoh_config)
    ```

    ```python
    # 控制主机2
    import os, json
    import zenoh
   
    # 获取当前文件所在目录路径
    current_dir = os.path.dirname(os.path.abspath(__file__))
    credentials_path = os.path.join(current_dir, "credentials-2.txt")

    # 初始化 zenoh 配置 （旧版本）
    # zenoh_config = zenoh.Config()

    # 初始化 zenoh 配置 （新版本）
    zenoh_config = zenoh.Config.from_json5(
        json=json.dumps(
            {
                "mode": "peer",
                "transport": {
                    "auth": {
                        "usrpwd": {
                            "user": "fourier-grx-2",  # 修改为匹配当前通信环境的 username
                            "password": "fourier-grx-2",  # 修改为匹配当前通信环境的 password
                            "dictionary_file": credentials_path,  # 修改为匹配目标 fourier-grx 的 credentials.txt 路径
                        }
                    },
                },
            }
        )
    )

    # 创建 zenoh 会话
    zenoh_session: zenoh.Session = zenoh.open(zenoh_config)
    ```

6. 重新运行程序，即可实现 Zenoh Peer 模式下的绑定。