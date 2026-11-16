# FEVM-FN60G-WE-Hackintosh

简体中文 · [English](./README.en.md)

![FEVM FN60G-WE 水冷版](https://cdn.mos.cms.futurecdn.net/MMkzeERxdT5ZmyTLJ44eYY-650-80.jpg.webp)

FEVM FN60G-WE 水冷版的 macOS Tahoe EFI，基于[黑果小兵的 FEVM-FN60G-Hackintosh](https://github.com/daliansky/FEVM-FN60G-Hackintosh)。公版下载见 [Releases](https://github.com/colorfulmeans/FEVM-FN60G-WE-Hackintosh/releases/latest)，仓库内路径为 [`V2/EFI`](./V2/EFI)。

## 配置清单

| 部件 | 型号与配置 |
| :--- | :--- |
| 主机 | FEVM FN60G-WE 水冷版 |
| CPU | Intel Core i5-12600T，6 核 12 线程 |
| 内存 | 32 GB DDR5-4800 |
| 核心显卡 | Intel UHD Graphics 770（Xe-LP），本 EFI 通过 `-wegnoigpu` 禁用 |
| 独立显卡 | AMD Radeon RX 6600M 8 GB，macOS 显示为 Radeon Pro W6600 |
| SSD | SK hynix PC711 1 TB NVMe |
| 无线网卡与蓝牙 | Broadcom BCM94360NG |
| 隔空投送 | 支持 |
| 有线网卡 | 双 Realtek RTL8125B 2.5GbE |
| 板载音频 | Realtek ALC897，layout-id 98 |
| SMBIOS | iMacPro1,1 |
| 系统与引导 | macOS Tahoe 26.7.3（25G342），OpenCore 1.0.8 MOD |

本机用 RX 6600M 输出和加速，保留 WhateverGreen。社区的 [WhateverXe](https://github.com/b00t0x/WhateverXe#installation) 已在 Alder Lake-S 的 UHD 770 上验证核显加速，项目文档要求 WhateverXe 和 WhateverGreen 分开使用。

## 实测

- 已验证启动、Recovery、背面 USB、3.5 mm 耳机、LAN2（2.5 Gb/s）、Wi‑Fi 和蓝牙耳机配对。
- RX 6600M 的 USB‑C ×1、HDMI ×1、DisplayPort ×2 均正常驱动。
- SIP 和 Authenticated Root 均已开启。板载音频使用 [SimpleHDA 1.0.1](https://github.com/laobamac/SimpleHDA/releases/tag/v1.0.1)，BCM94360NG 使用 [SimpleBCM 1.2.0](https://github.com/laobamac/SimpleBCM/releases/tag/v1.2.0)。
- 常规 H.264、HEVC 硬件编码测试通过。

## 生成自己的 SMBIOS 标识

将发布包中的 `EFI/`（或仓库中的 [`V2/EFI`](./V2/EFI)）复制到用于测试的启动 U 盘 EFI 分区。编辑复制后的 `EFI/OC/config.plist`，打开 `PlatformInfo → Generic`。公版的 `SystemSerialNumber`、`MLB`、`SystemUUID` 为空，`ROM` 为六个零字节。保留 `SystemProductName` 为 `iMacPro1,1`。

1. 解压 [OpenCore 1.0.8 RELEASE](https://github.com/acidanthera/OpenCorePkg/releases/tag/1.0.8)，在 `Utilities/macserial` 目录运行下面的命令。输出的 `|` 左边是 `SystemSerialNumber`，右边是 `MLB`。把同一行的两项成对填入，去掉两端空格。

   ```sh
   ./macserial --model iMacPro1,1 --num 1
   ```

2. 在终端运行 `uuidgen`，把输出填入 `SystemUUID`。
3. 运行 `networksetup -listallhardwareports`，查看本机物理有线网卡的接口和 MAC 地址。若选中的网口是 `en1`，也可以运行 `networksetup -getmacaddress en1`。取本机该网口的 MAC，去掉冒号，把 12 位十六进制数按六个字节填入 `ROM`，字段类型保持为 `Data`。
4. 保存到自己的启动 U 盘，确认可以启动后再登录 Apple 账户。四项标识仅保存在本机 EFI 和私人备份中，发布配置时使用公版占位值。

## 更新日志

### 2026-11-17 · v1.0.0

- 发布 FEVM FN60G-WE 水冷版 EFI，支持 macOS Tahoe 26.7.3（25G342）。

### 上游更新记录

- 7-30-2026

  - 更新 `OpenCore` `v1.0.8` MOD版
  - 支持 `Tahoe` 安装使用

- 2-25-2025

  - 更新 `OpenCore` `v1.0.4` 开发版
  - 重新定制了 USB 端口，支持所有的端口正常工作，修复 `LG Ultrafine 5K` 音频输入/输出及摄像头

- 9-22-2024

  - 更新 `OpenCore` `v1.0.2` 开发版
  - 更新 `IOSkywalkFamily.kext` 到 `v1.2.0`
  - 支持 `Sequoia` (博通无线网卡)
  - 支持 `Sonoma` (Intel无线网卡)

- 3-11-2024

  - 更新 `IOSkywalkFamily.kext` 到 `v1.1.0`
  - `Sonoma` 如果想更新到 `14.4` 请务必先更新 `EFI` ，然后再安装 [OCLP](https://pan.daliansky.net/APPS/OCLP/OCLP.md)，重启后，再升级到 `14.4` 否则会出现 `WIFI` 无法启用的问题

- 3-1-2024

  - 更新`OpenCore Mod` 到 `v0.9.9`

- 1-17-2024

  - 更新`OpenCore Mod` 到 `v0.9.8`
  - 修复超线程
  - 修复cpu型号显示异常
