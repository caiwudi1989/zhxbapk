# zhxbapk

Android APK、iOS IPA 打包环境与服务器接入规则。正式构建工作流及组件包待接入和验证。

## 构建环境

在本仓库 **Actions** 手动选择对应的环境工作流。GitHub 托管机器每次启动新环境，按工作流准备并检查下列工具。

| 工作流 | 托管环境 | 项目工具链 |
| --- | --- | --- |
| [Android environment](.github/workflows/android-environment.yml) | Windows Server 2025 | JDK 17、Gradle 8.5、SDK Platform 35、Build Tools 34.0.0 / 35.0.1、NDK r28c、Python 3.12、Node.js 20.18.1 |
| [iOS environment](.github/workflows/ios-environment.yml) | macOS 26 | Xcode 26.6、Swift、真机和模拟器 SDK、代码签名工具、Python 3.12、Node.js 20.18.1 |

两个工作流只准备和检查工具。Android 正式打包沿所选档位的 Windows 入口，iOS 归档和导出沿项目已有的 `ios_app/scripts/release_factory/release_factory.sh`。接入正式打包时，由授权的私有位置提供源码、组件及原签名材料。

## APP 支持组件

按实际打包档位核对 APP 宿主、AIW、脚本引擎、OCR、原生库及其他能力依赖。每个发布的组件记录版本、源码提交、工具链、ABI、依赖和 SHA-256。只把已核对可公开的组件发布到此仓库。客户项目材料、签名私钥和认证凭据保存在授权的私有位置。

## 服务器接入

1. PC 提交所选档位、当前脚本及资源，保留目录和相对路径。
2. 服务端沿用现有认证，保存材料，并在可运行的工作流接入后提交任务编号和材料引用。每次构建固定工作流版本、宿主版本及组件版本。
3. GitHub 托管机器取回构建输入，按现有正式入口完成资源合并、保护、混淆和签名。受保护调试包与客户 Release 沿各自身份处理。
4. 校验 APK 包名、版本、证书、保护清单及 SHA-256。服务端按任务编号取回本次结果并交付给 PC。

脚本或资源更新可以复用满足版本及摘要要求的 APP 组件。底层能力变更时，用最新源码重编所属宿主及全部直接和传递关联组件，并核对 APK 中实际装入的新产物。正式入口、签名或验包失败时，将该任务标为失败。
