# 初始化安卓手机网络

Hermes Agent 技能：通过 Appium MCP 和 ADB 连接安卓手机，按授权配置网络，并验证手机浏览器的出口。

## 安装

```sh
hermes skills install https://raw.githubusercontent.com/daniel-to-the-moon/android-network-initialization/main/SKILL.md
```

安装前请审阅 [SKILL.md](SKILL.md) 与相关权限。安装后可在 Hermes 中提出“初始化安卓手机网络”。

## 内容

- 检测 ADB、Node、Java、Android SDK 和 Hermes Appium MCP。
- 选择设备，创建 UiAutomator2 会话并验证设备信息、页面和截图。
- 在用户授权后导入代理配置，核对 VPN、IPv4、IPv6、WebRTC 和 DNS。
- 保护订阅地址和凭据，避免将正常连接误称为断线保护。

注意：Android 调试、VPN 和账户登录的授权应由设备所有者完成；代理不能单独保证零泄漏。该仓库不包含个人配置或凭据。

## 许可

目前未指定开源许可证。
