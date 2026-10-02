<div align="center">

<img src="https://raw.githubusercontent.com/kiriashi/BioPay/v2.1.2/images/app-icon.webp" width="160" alt="BioPay 应用图标" />

<h1>BioPay</h1>

<p>让支付应用轻松接入系统原生生物认证。</p>

[![Release](https://img.shields.io/github/v/release/kiriashi/BioPay?style=flat)](https://github.com/kiriashi/BioPay/releases)
[![Stars](https://img.shields.io/github/stars/kiriashi/BioPay?style=flat)](https://github.com/kiriashi/BioPay/stargazers)
[![Downloads](https://img.shields.io/github/downloads/Xposed-Modules-Repo/io.github.kiriashi.biopay/total?style=flat)](https://github.com/Xposed-Modules-Repo/io.github.kiriashi.biopay/releases)
[![License](https://img.shields.io/github/license/kiriashi/BioPay?style=flat)](https://github.com/kiriashi/BioPay/blob/v2.1.2/LICENSE)
[![Android](https://img.shields.io/badge/Android-10%2B-green.svg?style=flat)](https://developer.android.com)
[![LSPosed](https://img.shields.io/badge/LSPosed-API%20102-purple.svg?style=flat)](https://github.com/LSPosed/LSPosed)
[![Telegram](https://img.shields.io/badge/Telegram-交流群-blue.svg?style=flat)](https://t.me/biopaychat)

</div>


## 项目简介

**BioPay** 是一个面向 Android 10+、基于 LSPosed 的支付辅助模块。它支持注入微信、支付宝、淘宝、QQ 和云闪付的应用进程，识别到对应的支付窗口后，通过安卓标准的 **BiometricPrompt** 接口发起系统生物认证，并在验证成功后解密、自动无感输入预先保存的六位支付密码。
> [!WARNING]
> 部分手机的面容认证硬件仅达到便利级安全标准，被 Android 系统底层限制用于应用级认证，因而暂无法调用。

## 模块入口

| 应用名称 | 在哪里打开 BioPay 设置 |
| --- | --- |
| 微信 | 聊天页右上角 "+" 菜单 → "生物支付" |
| QQ | 聊天页右上角 "+" 菜单 → "生物支付" |
| 支付宝 | 我的 → 设置 → 支付设置 → 生物支付 |
| 淘宝 | 我的 → 设置 → 生物支付 |
| 云闪付 | 我的 → 设置 → 生物支付 |

页面识别和设置入口依赖支付应用的内部布局，可能随应用更新而变化。

## 安装与设置

**需要：** Android 10+ 版本、支持 LibXposed API 102 的 LSPosed，以及设备上已录入指纹或面容。

1. 从 [Releases](https://github.com/kiriashi/BioPay/releases)或 **模块仓库** 下载并安装 APK。
2. 在 LSPosed 中启用 BioPay，勾选需要使用的支付应用作用域。
3. 强制停止选中的应用，再重新打开。
4. 按上表进入相应应用的 BioPay 设置页。
5. 按需选择验证方式并设置支付密码，按提示完成系统验证并保存。
6. 下次付款时，按系统提示验证即可。

## 日常使用

- **切换验证方式：** 在对应应用的设置页调整指纹和面容开关，按提示验证后保存。已有密码时无需再次输入。
- **临时手动输入：** 点击取消验证按钮，或使用系统的导航栏按钮/手势进行返回。
- **重新验证：** 在支付键盘显示时按一次音量键可切换到生物认证页面。
- **关闭功能：** 在设置页关闭指纹和面容开关后保存，或是直接取消该应用的作用域勾选。
- **清除密码：** 长按设置页的清除按钮，按提示完成验证后清除。

## 支付流程

```mermaid
flowchart TD
    A[受支持的支付键盘出现] --> B[系统指纹或面容验证]
    B --> C{验证结果}
    C -->|通过| D[自动输入支付密码]
    C -->|取消或错误| E[恢复支付键盘]
    E --> F[手动输入密码]
    E -->|按音量键| B
    D --> G[由支付应用继续处理付款]
    F --> G
```

识别未通过时，可以继续尝试。BioPay 只协助输入密码，最终支付结果以支付应用提示为准。

## 隐私与安全

- BioPay 只声明一个 **使用生物特征硬件** 权限，不会申请其它网络、剪贴板等任何非必要权限。
- 模块也不主动收集、上传支付密码和生物信息，指纹和面容的识别都交由 Android 系统完成。
- 支付密码使用 AES-256-GCM 加密保存于本地，密钥由安卓 Keystore 统一管理，硬件保护能力取决于设备。
- 支付密码不会明文落盘，认证通过后才解密用于自动输入，使用后会立即清理明文缓冲区。

## 反馈与贡献

**问题反馈（Issue）**：
如果遇到 Bug 或有功能建议，欢迎提交 Issue。提交前请按以下步骤操作：
1. 从 [Releases](https://github.com/kiriashi/BioPay/releases) 下载 `-Debug.zip` 压缩包，解压后覆盖安装其中的 APK 并进行一遍问题复现，测试结束后覆盖安装正式版。
2. 在对应应用中打开「模块设置 → 异常诊断 → 导出」生成诊断日志，日志不会涉及任何个人敏感信息。
3. 进入 [Issues](https://github.com/kiriashi/BioPay/issues) 选择**问题报告(Bug Report)**，并附上诊断日志以及相关信息后进行提交。

**代码贡献（PR）**：
如果您已有不错的改进方案，也欢迎提交 Pull Request！无论是修复 Bug、优化现有逻辑，还是实现新功能。

## 致谢

* [FingerprintPay](https://github.com/eritpchy/FingerprintPay)
* [LSPosed](https://github.com/LSPosed/LSPosed)

## 开源协议与免责声明

Copyright (C) 2026 kiriashi。项目以 [GNU Affero General Public License v3.0 或更新版本](https://github.com/kiriashi/BioPay/blob/v2.1.2/LICENSE) 开源，修改和再分发须遵守该协议。

请仅在自己有权使用和修改的设备、账号上使用，并遵守相关法律及平台规则。本项目不提供任何担保；使用产生的账号、数据或支付风险由使用者自行承担。
