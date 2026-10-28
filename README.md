# MT7922 macOS Driver (Hackintosh)

**English:** Experimental macOS kernel extensions for the MediaTek MT7922 Wi-Fi/Bluetooth combo, targeting Hackintosh / macOS 13+ on x86_64 (Apple Silicon support is not implemented).

**中文：** 适用于 macOS（Hackintosh）的 MediaTek MT7922 Wi-Fi / 蓝牙实验性内核驱动，目标平台为 x86_64 的 macOS 13+。

> **⚠️ 免责声明 / Disclaimer**
> 本项目为实验性驱动，按原样提供，不提供任何形式的担保。内核扩展可能引发内核 panic、数据丢失或硬件异常。请仅在可接受风险的测试设备上使用，并确保能恢复系统。

---

## 支持硬件 / Supported Hardware

| 子系统 | 总线 | 匹配 ID |
|---|---|---|
| Wi-Fi | PCIe | `pci14c3,7922` / `pci14c3,0616` / `pci0b48,7922` |
| Bluetooth | USB | `0489:e0e2`（VID `0x0489` / PID `0xe0e2`） |

- Wi-Fi 同时支持“普通接口”与“原生菜单”两种模式，二者**只能加载其中一个**。
- Bluetooth 需要配合外置 [BlueToolFixup](https://github.com/acidanthera/BrcmPatchRAM) 与 [Lilu](https://github.com/acidanthera/Lilu) 使用。

---

## 功能状态 / Feature Status

### Wi-Fi（MT7922 / MT7922Native）
- ✅ PCIe 初始化、WFDMA、固件加载
- ✅ 2.4 GHz / 5 GHz 扫描与连接
- ✅ WPA2-PSK / CCMP
- ✅ 睡眠/唤醒电源管理恢复（v20，真机仍需验证）
- ⚠️ 6 GHz：仅支持被动发现，**不支持 6 GHz 联网**
- ⚠️ AWDL / AirDrop：仅在 Native 模式下识别接收动作帧，**不能实际传输文件**
- ❌ WPA3 / SAE / PMF / 企业级认证
- ❌ 160 MHz 带宽的真机性能尚未验证

### 蓝牙（MT7922Bluetooth）
- ✅ USB 固件上传与交接
- ✅ 设备限定的 HCI 随机地址补丁
- ⚠️ 需要外置 BlueToolFixup + Lilu 才能被系统正确识别

## 加载与使用 / Usage

### 依赖

- [KEXTLogKit.kext](https://github.com/acidanthera/KEXTLogKit)（所有 target 都需要）
- 蓝牙额外需要 `Lilu.kext` + `BlueToolFixup.kext`


### 日志开关

在对应 `Info.plist` 的 `IOKitPersonalities` 中修改 Boolean 值，重新编译并替换 kext：

| 驱动 | Info.plist 路径 | 键 | 默认 |
|---|---|---|---|
| MT7922 | `Source/MT7922/Info.plist` | `MT7922LogEnabled` | `false` |
| MT7922Native | `Source/MT7922/NativeWiFi/Info.plist` | `MT7922LogEnabled` | `false` |
| MT7922Bluetooth | `Source/Bluetooth/Info.plist` | `MT7922BluetoothLogEnabled` | `true` |

## 许可证 / License

本项目为**多许可证混合**，各组成部分许可证以其源文件头或对应 LICENSE 文件为准：

| 组成部分 | 许可证 | 说明 |
|---|---|---|
| 项目新增适配代码（`Source/MT7922` 多数文件、`Source/Bluetooth/Protocol` 等） | `ISC` | 项目独立实现 |
| `Source/Bluetooth/USB` | `GPL-2.0-only` | USB 服务实现 |
| `Source/Bluetooth/CompatLibrary` | `BSD-3-Clause` | 复用自 acidanthera/Lilu |
| `Source/Bluetooth/CompatLibrary/Decoder` | 专用 BSD 风格 | Hacker Disassembler Engine（HDE） |
| `Source/Bluetooth/Attribution/BlueToolFixup` 参考 | `GPL-2.0-only` | 原始参考来自 BrcmPatchRAM |
| `Source/mt76`、`Source/linux` | `BSD-3-Clause-Clear` / 文件头所示 | 参考/派生自 Linux mt76 |
| `Source/Bluetooth/Vendor/MacKernelSDK` | `Apple Public Source License 2.0` | Apple 内核 SDK |
| `Source/firmware/*.bin` | MediaTek 专有许可 | 见 `Source/mt76/firmware/LICENSE.mediatek` |
| `Source/MT7922/NativeWiFi/vendor/` | APSL / Apple 专有 | Apple 802.11 私有头参考 |

完整归属声明见 [`Source/Bluetooth/THIRD_PARTY.md`](Source/Bluetooth/THIRD_PARTY.md)。

**上传前请自行核对**：`Source/mt76` 与上游 Linux `mt76` 的 SPDX 声明是否一致；如果你从上游拉取了新的修改，务必保留并核对原许可证头。

---

## 致谢 / Credits

- Linux `mt76` / `mt7921` 驱动（MediaTek、Lorenzo Bianconi、Felix Fietkau 等）
- [acidanthera](https://github.com/acidanthera)：Lilu、BrcmPatchRAM / BlueToolFixup、MacKernelSDK
- [OpenIntelWireless/itlwm](https://github.com/OpenIntelWireless/itlwm)：原生 Wi-Fi 接口参考
- Apple IONetworkingFamily / IO80211Family（Apple Public Source License）

