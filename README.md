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

- Wi-Fi 同时支持“普通接口”与“原生菜单”两种模式。(普通接口已移除）
- Bluetooth 需要配合外置 [BlueToolFixup](https://github.com/acidanthera/BrcmPatchRAM) 与 [Lilu](https://github.com/acidanthera/Lilu) 使用。

---

## 功能状态 / Feature Status

### Wi-Fi（MT7922）
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

- [KEXTLogKit.kext](https://github.com/acidanthera/KEXTLogKit)（可能需要，仅在不能正常使用下尝试）
- 蓝牙额外需要 `Lilu.kext` + `BlueToolFixup.kext`

---

## 致谢 / Credits

- Linux `mt76` / `mt7921` 驱动（MediaTek、Lorenzo Bianconi、Felix Fietkau 等）
- [acidanthera](https://github.com/acidanthera)：Lilu、BrcmPatchRAM / BlueToolFixup、MacKernelSDK 无相关内容引用，仅做系统修补


