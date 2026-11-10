---
name: initialize-android-network
description: Use when initializing Android network via Appium MCP.
version: 1.0.0
author: daniel-to-the-moon
license: NOASSERTION
metadata:
  hermes:
    tags: [android, appium, mcp, adb, network, vpn]
    related_skills: [hermes-agent, network-connectivity]
---

# 初始化安卓手机网络

## When to Use

Use this skill to connect Hermes Agent to a user's Android device through Appium MCP, inspect the actual device, configure an authorized proxy, and verify its network. Use the active Hermes profile only. Never assume the computer's VPN automatically applies to the phone.

## Safety and privacy

- Ask for consent before device control, installing apps or changing VPN/routing. The user personally approves USB debugging, Android VPN dialogs, login/2FA and payments. Never collect passwords, OTPs, proxy credentials or subscription URLs in chat, screenshots, logs or public repositories.
- Do not turn on airplane mode, disable the existing network, revoke USB authorization, reset the phone, alter GPS/region/timezone, change store country, or delete personal files unless explicitly requested. Configuration changes may temporarily expose direct traffic; a working proxy is not a kill switch.
- Inspect remote scripts and download sources, use upstream releases and verify APK identity/digest when available. Keep sensitive temporary files private; remove only artifacts created for the task after successful import. Never publish device serials, local account data, real IP addresses, credentials, private URLs, screenshots, configuration exports or logs.

## 1. Discover and prepare

1. Check phone, host OS, available disk and installed tools; locate Android SDK and `adb` rather than hard-coding a user path. On macOS a common SDK location is `$HOME/Library/Android/sdk`. Run `adb version` and `adb devices -l`; do not publish serials. If absent or `unauthorized`, have the user unlock, connect with a data-capable cable, enable Developer options and USB debugging, and personally approve the RSA prompt. Multiple devices: ask which to control. If disconnected, stop and reconnect; a listed stale Appium session is not proof of a connected device.
2. Discover Java, Node, Android SDK/platform-tools and existing Appium prerequisites, including the PATH and environment seen by the MCP server. Use `mcp__appium__appium_skills` with `platform=android`, `driver=uiautomator2` and setup or troubleshoot mode for current requirements. Check official Android/Appium sources; don't blindly install extra emulators or change existing Java. Get user approval for installations.
3. Compare the Node, Java and SDK paths actually passed to the MCP subprocess with `node -v` and `java -version` in the shell: a different system-default runtime can make a working MCP installation look broken. Resolve the server entry point from its installed package or the current catalog rather than guessing its build path; diagnose compatibility errors before changing versions.
4. Check Hermes MCP status via `hermes mcp list` and `hermes mcp test appium`. If Appium is absent, use the Hermes connection manager for the catalog Appium MCP (`manage_connections`, action `install`, target `{name: appium, mcp: true}`), or the current documented interactive catalog. Do not hand-edit `mcp_servers`; avoid saving secrets in configuration. Enable only the needed device selection, session, info, screenshot, page-source, element, gesture and window-size tools. Read the current schemas through `tool_describe` because versions vary.

## 2. Create and validate an Appium session

1. For a local MCP server, call `mcp__appium__select_device` with `{"platform":"android"}`. One device may be automatically selected; with several, obtain the user's choice and pass its `deviceUdid` privately. For local embedded mode do not invent a `remoteServerUrl` or launch a duplicate server. Only use remote mode when the user supplies the actual server URL; follow the tool schema and skip local selection.
2. Call `mcp__appium__appium_session_management` with `{"action":"create","platform":"android"}` and, when needed, `capabilities` as a JSON string containing `appium:udid` for the selected device. Then use `appium_mobile_device_info` with `{"action":"info"}`, and `appium_get_page_source` or `appium_screenshot`. Check active session and actual displayed screen before actions. Record only non-sensitive model/OS and test outcome.
3. Use accessibility selectors and screenshots to operate the device, re-read state after each state-changing action. Black screenshots do not prove the device is locked or broken; check page source, display state and device connection. Do not promise a low-frame-rate screenshot loop is a responsive live video stream. Delete only the session created for this task when done, if appropriate.

## 3. Configure the Android network only if requested

1. Read the current active VPN, proxy app, selected profile, mode and node; record enough to restore without exposing secrets. Determine the requested target and actual upstream chain, not just a matching node name, a general client configuration export or a country label.
2. If installing a proxy client or importing a subscription, obtain consent and use its official compatible package and documented import flow. Treat URLs and node credentials as secrets; do not print them or publish temporary files. Prefer app-supported import. If creating a generated snapshot, explicitly disclose that it won't automatically track subscription changes. Preserve the original source and settings.
3. For an explicitly requested SOCKS5 exit through a VLESS proxy, a compatible Mihomo profile can use the SOCKS5 outbound's `dialer-proxy` pointing to the exact upstream outbound name. Confirm upstream and protocols from the actual configuration and provider; never insert real credentials into a published skill. Check the app's selected node and mode before enabling VPN: global mode may initially select `DIRECT`. Have the user approve the system VPN prompt and verify the actual selected node and Android active VPN afterwards.
4. Import helpers must use supported interpreter/library versions and supported Android intent options. Check exit status and device UI after each attempt; a successful local HTTP service alone doesn't prove app import. Record any ADB reverse mapping or local server created for the import and remove only those task-owned resources after the phone has imported the profile; stale mappings can leave unexpected access paths. Avoid logging complete configuration or authentication.

## 4. Verify and report

- Test on the phone's real browser, not the host browser: independently record observed webpage IPv4, IPv6, WebRTC and DNS results, active VPN, actual node/mode, and the target site. Compare with the requested expected exit without publishing personal addresses. A generic IP lookup does not prove every app or third-party login uses that path.
- Recheck after authorized changes and report time of test, exact scope and failures. Explicitly distinguish observed normal operation from untested VPN restart, device reboot, captive networks, provider expiry and network loss. Do not claim a fail-closed kill switch unless Always-on VPN / block-without-VPN behavior is configured, compatible with the app, and tested with informed consent. A phone's location, language, timezone and store eligibility are not proven by its IP.
- Never trigger a disconnection/fault test without informed consent and a local recovery plan. If verification cannot be performed, state the blocker rather than inventing output.
