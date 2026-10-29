# MT7922 macOS Driver (Hackintosh)
仓库管理员 qiqco，软件撰写者 Angus666888，驱动测试QQ群708393964

**English:** Experimental macOS kernel extensions for the MediaTek MT7922 Wi-Fi/Bluetooth combo, targeting Hackintosh / macOS 13+ on x86_64 (Apple Silicon support is not implemented).

**中文：** 适用于 macOS（Hackintosh）的 MediaTek MT7922 Wi-Fi / 蓝牙实验性内核驱动，目标平台为 x86_64 的 macOS 13和26。

！！！！！github提供版本仅确保支持26，其他版本需自行尝试，os13驱动可以至q群询问！！！！！
关于我们的特色，在文末看看-jump to the end of file to view the overall project

> ** 免责声明 / Disclaimer**
> 本项目为实验性驱动，按原样提供，不提供任何形式的担保。内核扩展可能引发内核 panic、数据丢失或硬件异常。请仅在可接受风险的测试设备上使用，并确保能恢复系统。

---

## 支持硬件 / Supported Hardware

| 子系统 | 总线 | 匹配 ID |
|---|---|---|
| Wi-Fi | PCIe | `pci14c3,7922` / `pci14c3,0616` / `pci0b48,7922` |
| Bluetooth | USB | `0489:e0e2`（VID `0x0489` / PID `0xe0e2`） |

- Wi-Fi 同时支持“普通接口”与“原生菜单”两种模式。(普通接口已移除）【os13】，当前研发主线在MACOS26，13版本有可运行驱动，但是暂时不再维护
- Bluetooth 需要配合外置 [BlueToolFixup](https://github.com/acidanthera/BrcmPatchRAM) 与 [Lilu](https://github.com/acidanthera/Lilu) 使用（理论支持所有（至少10.13）系统，13和26下蓝牙已经测试无误）。

---

## 功能状态 / Feature Status

### Wi-Fi（MT7922 OS26）
目前除了6G明确不能连接，可能可以扫描，暂无明显问题。
关于性能：制作者，我，仅流量环境，最大5G80mhz，可能存在速率上限，暂时无法测试，暂时不修复，修复隔空投送中。

### 蓝牙（MT7922Bluetooth）
- USB 固件上传与交接
- 设备限定的 HCI 随机地址补丁
- 需要外置 BlueToolFixup + Lilu 才能被系统正确识别

## 加载与使用 / Usage

### 依赖
WIFI
- 直接加载使用即可，无需任何修补，纯原生
- [KEXTLogKit.kext](https://github.com/acidanthera/KEXTLogKit)（可能需要，理论上不需要，仅在不能正常使用下尝试）
蓝牙
- 蓝牙额外需要 `Lilu.kext` + `BlueToolFixup.kext`

---

## WIFI部分致谢 / Credits
- Linux mt76项目
- （注：本项目独立开发，与市面上现有所有MT7922均无参考关系，唯一参考为mt76，为确保开发者更新性质暂时不开源源码！！）

  
## 蓝牙部分致谢 / Credits 
- [acidanthera](https://github.com/acidanthera)：Lilu、BrcmPatchRAM / BlueToolFixup、MacKernelSDK 无相关内容引用，仅做系统修补，但为高版本使用必备，特此致谢
- 开发工程中遇到一些问题，在intelBTpatch项目中找到灵感，所以也在此处致谢，无相关代码引用，使用。

## 关于这个驱动
<img width="508" height="376" alt="image" src="https://github.com/user-attachments/assets/805d77a8-5f70-4249-bfd4-28a5ef6b23d9" />
ps：你可能无法在使用release/仓库中的驱动中看到awdl项目，awdl正在测试中
纯skywalk接入apple原生射频管理，并未使用intelwifi入网渠道，目前，驱动反响良好
关于版本支持，目前macos26为主线，os13为过去支持版本，os13下同样使用skywalk管理射频，但是接入io80211family（好像是），本项目的目的是实现和AppleBCMWlanCore系列完全一致的架构和驱动级别，以此实现纯原生wifi管理。其他详细细节请进群了解，我想不出来了完善隔空投送去了，目前扫描已成功。
