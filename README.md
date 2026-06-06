# OpenCore EFI for Huawei MateStation B515

[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

华为 MateStation B515 黑苹果 OpenCore 引导 EFI，已成功安装 **macOS Sonoma**。

## 硬件配置

| 组件 | 型号 | 状态 |
|------|------|------|
| CPU | AMD Ryzen 5 4600G (Renoir, 6C/12T) | ✅ |
| iGPU | AMD Radeon Graphics (Vega 6) | ✅ NootedRed 驱动，性能受 AMD 补丁限制 |
| 主板 | B550 芯片组 (MateStation B515 OEM) | ✅ |
| 内存 | DDR4 | ✅ |
| 网卡 | Realtek RTL8111 | ✅ |
| 无线/蓝牙 | PCIe 无线网卡 / Intel 蓝牙 | ✅ |
| 声卡 | Realtek ALC | ✅ AppleALC |
| USB | B550 芯片组 | ✅ (一个 Type-C 口不可用) |
| NVMe | NVMe SSD | ✅ NVMeFix |

##  OpenCore 版本

| 项目 | 值 |
|------|-----|
| OpenCore | 1.0.6 |
| SMBIOS | MacPro7,1 |
| 目标 macOS | Sonoma (14.x) |
| NVRAM | 已包含 AML 补丁 |

## Kexts 列表

| Kext | 版本 | 功能 |
|------|------|------|
| [NootedRed](https://github.com/ChefKissInc/NootedRed) | latest | AMD Renoir iGPU 驱动 |
| [AMDRyzenCPUPowerManagement](https://github.com/trulyspinach/SMCAMDProcessor) | latest | AMD CPU 电源管理 |
| [SMCAMDProcessor](https://github.com/trulyspinach/SMCAMDProcessor) | latest | AMD 传感器监控 |
| [AmdTscSync](https://github.com/naveenkrdy/AmdTscSync) | latest | AMD TSC 同步（修复内核 panic） |
| [Lilu](https://github.com/acidanthera/Lilu) | latest | 内核扩展补丁框架 |
| [VirtualSMC](https://github.com/acidanthera/VirtualSMC) | latest | SMC 模拟 |
| [WhateverGreen](https://github.com/acidanthera/WhateverGreen) | latest | GPU 修复与优化 |
| [AppleALC](https://github.com/acidanthera/AppleALC) | latest | 板载音频 |
| [RestrictEvents](https://github.com/acidanthera/RestrictEvents) | latest | 事件限制（禁用内存报错等） |
| [NVMeFix](https://github.com/acidanthera/NVMeFix) | latest | NVMe 电源管理修复 |
| [AirportItlwm](https://github.com/OpenIntelWireless/itlwm) | Sonoma 14.4 | Intel 无线网卡 |
| [IntelBluetoothFirmware](https://github.com/OpenIntelWireless/IntelBluetoothFirmware) | latest | Intel 蓝牙固件 |
| [BlueToolFixup](https://github.com/acidanthera/BrcmPatchRAM) | latest | 蓝牙修复 |
| [IntelBTPatcher](https://github.com/OpenIntelWireless/IntelBluetoothFirmware) | latest | Intel 蓝牙补丁 |
| [RealtekRTL8111](https://github.com/RehabMan/OS-X-Realtek-Network) | latest | 板载有线网卡 |
| [USBInjectAll](https://github.com/RehabMan/OS-X-USB-Inject-All) | latest | USB 注入 |
| [USBMap](https://github.com/corpnewt/USBMap) | custom | USB 端口定制 |
| [AppleMCEReporterDisabler](https://github.com/acidanthera/AppleMCEReporterDisabler) | latest | 禁用 MCE 报告（AMD 需要） |

## BIOS 设置

进入 BIOS（开机按 Del/F2），确保以下设置：

| 选项 | 设置 |
|------|------|
| Secure Boot | **Disabled** |
| Fast Boot | **Disabled** |
| CSM | **Disabled** (UEFI only) |
| SATA Mode | **AHCI** |
| Above 4G Decoding | **Enabled** |
| Resizable BAR | **Disabled** |
| AMD SVM | **Enabled** |
| IOMMU | **Enabled** |
| UMA Frame Buffer Size | **2G** 或以上（给 iGPU 分配显存） |

## 首次使用

### 1. 生成唯一 SMBIOS

此 EFI 使用 **MacPro7,1** SMBIOS，但序列号等需要自行生成（否则 iMessage/FaceTime 等无法使用）：

```bash
# 使用 GenSMBIOS 工具
# https://github.com/corpnewt/GenSMBIOS
# 选择 MacPro7,1，生成 Serial / Board Serial / SmUUID / ROM
```

将生成的数值填入 `config.plist` → `PlatformInfo → Generic`：
- `SystemProductName`: MacPro7,1
- `SystemSerialNumber`: (生成的序列号)
- `MLB`: (生成的 Board Serial)
- `SystemUUID`: (生成的 SmUUID)
- `ROM`: (网卡 MAC 地址)

### 2. 制作安装盘

```bash
# 在 macOS 下制作安装盘
# 下载 Sonoma 安装程序，然后：
sudo /Applications/Install\ macOS\ Sonoma.app/Contents/Resources/createinstallmedia --volume /Volumes/MyUSB

# 将 EFI 复制到 USB 的 ESP 分区
# 挂载 USB 的 EFI 分区 → 将本仓库的 EFI 文件夹复制进去
```

### 3. 安装 macOS

1. 插入制作好的安装 U 盘
2. 开机按 F11/F12 选择 UEFI 启动项 → OpenCore
3. 选择 `Install macOS Sonoma`
4. 按照安装流程操作（格式化硬盘为 APFS）
5. 安装完成后将 EFI 复制到本地硬盘的 ESP 分区
   ```bash
   # 在 macOS 终端中
   sudo diskutil mount EFI
   # 复制 EFI 文件夹到挂载的 EFI 分区
   ```

## 专用启动参数

此 EFI 已配置以下 **boot-args**：

| 参数 | 说明 |
|------|------|
| `-v` | 详细启动模式（可删除） |
| `alcid=xx` | 音频 layout-id（已配置） |
| `npci=0x2000` | AMD 平台 PCI 配置修复 |
| `radpg=15` | AMD GPU 补丁 |
| `amfi_get_out_of_my_way=1` | 跳过 AMFI 签名验证 |
| `-amd_no_dgpu_accel` | 如有独立显卡时需要 |

### 安装后调整

参考 `config-after-post.plist`，其中包含安装完成后的优化配置（如 USB 映射微调等）。

## 注意事项

- **必须使用本仓库的 EFI**，通用 AMD OpenCore 配置无法正常启动华为 MateStation B515
- AMD 平台无法使用 Adobe 全家桶、VMware Fusion 等依赖 Intel 虚拟化的软件
- iGPU 性能不如原生，游戏和专业图形工作建议使用独立显卡
- **每次升级 OpenCore/kexts 前备份当前 EFI**

## 已知问题

- AMD iGPU (NootedRed) 性能受限，图形性能约为原生的一半
- 一个 Type-C USB 口不可用（物理限制定位问题）
- 隔空投送 (AirDrop) 需更换原生苹果无线网卡
- 无法使用 Sidecar（AMD 平台限制）
- 睡眠/唤醒基本正常

## 更新 EFI

建议定期更新 kexts 和 OpenCore 版本：

```bash
# 使用 OC Auxiliary Tools 或 ProperTree 同步更新
# 或手动替换 kexts + OpenCore.efi + BOOTx64.efi
# 更新后务必用 OCValidate 检查 config.plist
```

## 鸣谢

- [OpenCore 官方](https://github.com/acidanthera/OpenCorePkg)
- [NootedRed](https://github.com/ChefKissInc/NootedRed)
- [AMD OS X 社区](https://amd-osx.com)
- [acidanthera 团队](https://github.com/acidanthera)
- [Dortania 黑苹果安装指南](https://dortania.github.io/OpenCore-Install-Guide/)
