# OpenCore EFI for Huawei MateStation B515

[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

华为 MateStation B515 黑苹果 OpenCore 引导 EFI。已成功安装 **macOS Sonoma**。

## 硬件配置

| 组件 | 型号 | 状态 |
|------|------|------|
| CPU | AMD Ryzen 4600G | ✅ |
| 主板 | B550 芯片组 | ✅ |
| iGPU | AMD Radeon Graphics (Vega 6) | ✅ NootedRed 驱动，性能受 AMD 补丁限制 |
| 网卡 | Realtek RTL8111 | ✅ |
| 无线 | PCIe 无线网卡 | ✅ |
| 蓝牙 | Intel 蓝牙 | ✅ |
| USB | B550 芯片组 | ✅ (一个 Type-C 口不可用) |

## OpenCore 版本

- **OpenCore**: 1.0.6
- **SMBIOS**: MacPro7,1
- **macOS**: Sonoma

## Kexts 列表

- [NootedRed](https://github.com/ChefKissInc/NootedRed) — AMD iGPU 驱动
- [AMDRyzenCPUPowerManagement](https://github.com/trulyspinach/SMCAMDProcessor) — AMD CPU 管理
- [AmdTscSync](https://github.com/naveenkrdy/AmdTscSync) — AMD TSC 同步
- [SMCAMDProcessor](https://github.com/trulyspinach/SMCAMDProcessor) — AMD 传感器
- [AppleALC](https://github.com/acidanthera/AppleALC) — 音频
- [Lilu](https://github.com/acidanthera/Lilu) — 内核补丁
- [VirtualSMC](https://github.com/acidanthera/VirtualSMC) — SMC 模拟
- [WhateverGreen](https://github.com/acidanthera/WhateverGreen) — GPU 修复
- [AirportItlwm](https://github.com/OpenIntelWireless/itlwm) — Intel 无线网卡
- [IntelBluetoothFirmware](https://github.com/OpenIntelWireless/IntelBluetoothFirmware) — Intel 蓝牙
- [BlueToolFixup](https://github.com/acidanthera/BrcmPatchRAM) — 蓝牙修复
- [IntelBTPatcher](https://github.com/OpenIntelWireless/IntelBluetoothFirmware) — Intel 蓝牙补丁
- [RealtekRTL8111](https://github.com/RehabMan/OS-X-Realtek-Network) — 有线网卡
- [USBInjectAll](https://github.com/RehabMan/OS-X-USB-Inject-All) — USB 注入
- [USBMap](https://github.com/corpnewt/USBMap) — USB 定制
- [NVMeFix](https://github.com/acidanthera/NVMeFix) — NVMe 修复
- [RestrictEvents](https://github.com/acidanthera/RestrictEvents) — 事件限制
- [AppleMCEReporterDisabler](https://github.com/acidanthera/AppleMCEReporterDisabler) — MCE 报告禁用

## 使用说明

1. 将 EFI 文件夹复制到 ESP 分区
2. 使用 OpenCore 引导启动
3. 如需安装后调整 USB，参考 `config-after-post.plist`

### 注意

**必须使用本仓库的 EFI**，因为已添加了 AMD 专用补丁，其他通用 EFI 无法正常启动。

## 已知问题

- AMD iGPU 性能受限（NootedRed 驱动自身限制）
- 一个 Type-C USB 口不可用
- 需要使用 PCIe 无线网卡或 Intel 无线方案

## 鸣谢

- [OpenCore 官方](https://github.com/acidanthera/OpenCorePkg)
- [NootedRed](https://github.com/ChefKissInc/NootedRed)
- [AMD OS X 社区](https://amd-osx.com)
