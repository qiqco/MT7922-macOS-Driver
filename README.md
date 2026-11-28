# MT7922 macOS Driver (Hackintosh)

> **仓库管理员：** qiqco ｜ **作者：** Angus666888 ｜ **驱动测试 QQ 群：** `708393964`  
> 非常感谢 **Xelon233** 送来 Token 支持！

**English:** Experimental macOS kernel extensions for the MediaTek MT7922 Wi-Fi/Bluetooth combo, targeting Hackintosh / macOS 13 and 26.5+ on `x86_64` (Apple Silicon support is not implemented).

**中文：** 适用于 macOS（Hackintosh）的 MediaTek MT7922 Wi-Fi / 蓝牙实验性内核驱动，目标平台为 `x86_64` 架构的 macOS 13 与 macOS 26。

超级重要
本驱动蓝牙分离，蓝牙库见：https://github.com/angus666888/MT7922Bluetooth
下述蓝牙均为旧版本，不做使用

> [!IMPORTANT]
> - GitHub 提供的版本**仅确保支持 macOS 26.5 及以上**（编译 SDK 使用 26.5），26 较旧版本不做专门适配（可自行尝试），后续会持续跟进 macOS 26 最新系统。
> - macOS 13 驱动暂不再维护，如需获取可进 QQ 群询问。
> - 由于近期更新频繁，GitHub 仓库可能不是最新版本，建议加群获取最新测试版。
> - 本驱动（MT7922AirLink）目前问题仅为网络连接协议，手上缺少多种握手协议wpa/wpa2等，无法进行测试，（MT7922SkyWalk）驱动更新需谨慎

> [!WARNING]
> **免责声明 / Disclaimer**  
> 本项目为实验性驱动，按原样提供，不提供任何形式的担保。内核扩展可能引发内核 Panic、数据丢失或硬件异常。请仅在可接受风险的测试设备上使用，并确保具备恢复系统的手段。

---

## 最新动态 / Updates

### 11/28 更新
- **无阻塞全向隔空投送（AirDrop）：** 已支持在保持正常网络连接的同时使用隔空投送。
- **关于 Panic（已修复）：** 现在在 macOS 下正常加载网卡后，可直接替换 EFI 中的本驱动并**热重启**生效。
  - ~~旧版已知问题（已修复，可忽略）：进入 macOS 时有约 1/5 ~ 1/20 概率发生 Panic，推测与固件加载卸载有关，此前需断电重启解决。~~

<details>
<summary>历史测试记录（点击展开）</summary>

- ~~第 330 版 Wi-Fi 测试，`v1.2.64`~~
- ~~QUIC 建联 < 0.000s / TLS 1.3 握手 0.171s / 往返 RTT 0.007s，无重连、无乱序~~
- ~~`awdl0` 实测速率：最低 0.6 MB/s，最大 1.7 MB/s，中位数 1.4 MB/s~~

</details>

---

## 支持硬件 / Supported Hardware

| 子系统 | 总线 | 匹配 ID |
|---|---|---|
| Wi-Fi | PCIe | `pci14c3,7922` / `pci14c3,0616` / `pci0b48,7922` |
| Bluetooth | USB | `0489:e0e2`（VID `0x0489` / PID `0xe0e2`） |

- **Wi-Fi：** 当前研发主线为 macOS 26（原生菜单模式）。macOS 13 曾支持普通接口与原生菜单，目前已移除普通接口且暂停维护。
- **Bluetooth：** 需配合外置 [BlueToolFixup](https://github.com/acidanthera/BrcmPatchRAM) 与 [Lilu](https://github.com/acidanthera/Lilu) 使用。理论支持 macOS 10.13 及以上所有系统，macOS 13 和 26 下已测试无误。

---

## 功能状态 / Feature Status

### Wi-Fi（MT7922 macOS 26）
- 除 **6GHz 频段**明确无法连接（可能可扫描）外，目前暂无明显问题。
- **隔空投送（AirDrop）：** 已修复并支持无阻塞传输。
- **关于性能：** 作者目前仅在流量环境下测试，最大环境为 5GHz 80MHz，可能存在速率上限，暂无法测试与修复。

### 蓝牙（MT7922Bluetooth）
- 支持 USB 固件上传与交接。
- 内置设备限定的 HCI 随机地址补丁。
- 需配合外置 `BlueToolFixup` + `Lilu` 才能被系统正确识别。

---

## 加载与使用 / Usage

### Wi-Fi
- 直接加载使用即可，无需任何修补，纯原生接入。
- ~~[KEXTLogKit.kext](https://github.com/acidanthera/KEXTLogKit)（不需要，仅在无法正常使用时尝试）~~

### 蓝牙
- 必需依赖：`Lilu.kext` + `BlueToolFixup.kext`
- **加载顺序：** `Lilu.kext` → `BlueToolFixup.kext` → 本驱动
- ~~[KEXTLogKit.kext](https://github.com/acidanthera/KEXTLogKit)（一般不需要，仅在无法正常使用时尝试）~~

---

## 关于这个驱动 / About

<img width="508" height="376" alt="驱动架构示意" src="https://github.com/user-attachments/assets/805d77a8-5f70-4249-bfd4-28a5ef6b23d9" />

> 注：Release 或仓库中的旧版驱动可能暂未显示 `awdl0` 项目，AWDL 功能正在持续完善中。

本项目采用**纯 Skywalk 接入 Apple 原生射频管理**，未使用 Intel Wi-Fi 入网渠道，目前反馈良好。

- **版本架构：** 目前以 macOS 26 为主线；macOS 13 同样使用 Skywalk 管理射频，但接入 `IO80211Family`。
- **项目目标：** 实现与 `AppleBCMWlanCore` 系列完全一致的架构和驱动级别，达成纯原生 Wi-Fi 管理。更多细节欢迎进群交流。

---

## 常见问题与自助排查 / FAQ

### 1. 系统信息里全是 `<redact>` 怎么办？是不是驱动没做好？
这是因为 Skywalk Wi-Fi 自带信息脱敏与 SSID 隐藏机制。如果你不介意暴露日志信息，可以在 `boot-args` 中加入：
```text
corewifi_redact_logs=0
