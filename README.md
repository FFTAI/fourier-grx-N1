# fourier-grx-N1

Fourier GRX N1 SDK 文档站点仓库。

该仓库主要维护 GitHub Pages / Jekyll 文档内容，用于说明 Fourier-GRX-N1 SDK 的安装、运行模式、接口、任务说明和常见问题；它不是 SDK 源码仓库。

## 主要入口

- 文档首页：`index.md`
- 站点配置：`_config.yaml`
- 快速开始：`docs/quickstart.md`
- 示例代码说明：`docs/examples.md`
- 参考指南：`docs/reference.md`
- 任务说明：`docs/tasks.md`
- 常用操作：`docs/usage.md`
- 常见问题：`docs/faq.md`
- 固件更新：`docs/update.md`

## 文档结构

- `docs/quickstart/`：按机器人类型组织的入门内容
- `docs/examples/`：User / Developer / PubSub 接口示例说明
- `docs/reference/`：接口协议、运行模式、配置文件、系统运行逻辑等参考文档
- `docs/tasks/`：任务与模块说明，包含任务值（TID）和模块值（MID）
- `docs/training/`：训练相关文档
- `docs/user_case/`：典型使用场景
- `assets/images/`：文档图片资源

## 相关说明

- 示例代码主要位于外部仓库 `Wiki-GRx-Deploy` 的 `FourierN1` 分支，本仓库主要负责文档说明。
- 页面普遍使用站点绝对路径，如 `/fourier-grx-N1/docs/...` 与 `/fourier-grx-N1/assets/...`。
