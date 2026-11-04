# MT7922 macOS Driver (Hackintosh)
仓库管理员 qiqco，软件撰写者 Angus666888，驱动测试QQ群708393964，第330版wifi测试，v1.2.64

## 关于panic
现在在开机（macos）下正常加载网卡后，直接替换efi本驱动，直接使用！热重启！进入macos时候有概率panic（约1/5-1/20几率），暂不考虑修复，推测可能和固件加载卸载有关。解决方法：完成一次断电开机就可以了！
##

**English:** Experimental macOS kernel extensions for the MediaTek MT7922 Wi-Fi/Bluetooth combo, targeting Hackintosh / macOS 13，26.5+ on x86_64 (Apple Silicon support is not implemented).

**中文：** 适用于 macOS（Hackintosh）的 MediaTek MT7922 Wi-Fi / 蓝牙实验性内核驱动，目标平台为 x86_64 的 macOS 13和26。

！！！！！github提供版本仅确保支持26.5以上，其他版本需自行尝试，os13驱动可以至q群询问！！！！！
26最新系统一定会支持，26较旧系统不会去支持，可能可以使用，编译sdk使用26.5

关于我们的特色，在文末看看-jump to the end of file to view the overall project

##更多的志同道合的朋友的项目集

--https://github.com/Xelon233/Vela-X
##

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
- [KEXTLogKit.kext](https://github.com/acidanthera/KEXTLogKit)（不需要，仅在不能正常使用下尝试）
蓝牙
- 蓝牙额外需要 `Lilu.kext` + `BlueToolFixup.kext`
- [KEXTLogKit.kext](https://github.com/acidanthera/KEXTLogKit)（可能需要，仅在不能正常使用下尝试）
- 加载顺序kextlogkit/bluetoolfixup（同级）先加载，后本驱动
---

## WIFI部分致谢 / Credits
- Linux mt76项目
- （注：本项目独立开发，与市面上现有所有MT7922均无参考关系，唯一参考为mt76，为确保开发者更新兴致暂时不开源源码！！）

  
## 蓝牙部分致谢 / Credits 
- [acidanthera](https://github.com/acidanthera)：Lilu、BrcmPatchRAM / BlueToolFixup、MacKernelSDK 无相关内容引用，仅做系统修补，但为高版本使用必备，特此致谢
- 开发工程中遇到一些问题，在intelBTpatch项目中找到灵感，所以也在此处致谢，无相关代码引用，使用。

## 关于这个驱动

<img width="508" height="376" alt="image" src="https://github.com/user-attachments/assets/805d77a8-5f70-4249-bfd4-28a5ef6b23d9" />
ps：你可能无法在使用release/仓库中的驱动中看到awdl项目，awdl正在测试中

纯skywalk接入apple原生射频管理，并未使用intelwifi入网渠道，目前，驱动反响良好

关于版本支持，目前macos26为主线，os13为过去支持版本，os13下同样使用skywalk管理射频，但是接入io80211family（好像是），本项目的目的是实现和AppleBCMWlanCore系列完全一致的架构和驱动级别，以此实现纯原生wifi管理。其他详细细节请进群了解，我想不出来了完善隔空投送去了，目前扫描已成功。

## 问题自我解决指南：

1.我在系统信息里面看到全是<redact>什么的怎么办？肯定是你驱动没做好！！？
解：shkywalkwifi信息去敏，自带ssid隐藏，如果你无惧风险继续查看，那么在boot-args加入 corewifi_redact_logs=0；ps：这不是我想加的，为什么是这个找Apple问去。
2.我加载了驱动，但是发现不能用，ioregistryexplorer显示没有任何和mt7922相关项目挂载。
：新款电脑，尤其是初始网卡支持6G频段的电脑通常acpi表会通过sar关闭6G电源，但是sar有多种配置，且就算是某些大厂的配置也乱七八糟的，不一定sar配置被支持，如果想获得纯原生体验，你可以提交issue，如果单纯想要解锁全部功能和日用/懒得提交，你可以在infoplist自行修改MT7922IgnorePlatformSAR项目，忽略平台acpi配置一般可以解决。
4.如果你的电脑出现反复重启无法打开wifi：
：去win一下，关机在进macOS，暂不清楚成因，如有突破或者有找到成因者欢迎在issue留言
3.关于开源
：如果你愿意补齐30亿token（6000美刀）+1200元dpsk的花费，我可以把源码交由于你。
