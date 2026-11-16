# FEVM-FN60G-WE-Hackintosh

[简体中文](./README.md) · English

![Liquid-cooled FEVM FN60G-WE](https://cdn.mos.cms.futurecdn.net/MMkzeERxdT5ZmyTLJ44eYY-650-80.jpg.webp)

macOS Tahoe EFI for the liquid-cooled FEVM FN60G-WE, based on [daliansky’s FEVM-FN60G-Hackintosh](https://github.com/daliansky/FEVM-FN60G-Hackintosh). Download the public EFI from [Releases](https://github.com/colorfulmeans/FEVM-FN60G-WE-Hackintosh/releases/latest) or find it under [`V2/EFI`](./V2/EFI) in this repository.

## Hardware

| Component | Model and configuration |
| :--- | :--- |
| System | FEVM FN60G-WE, liquid-cooled edition |
| CPU | Intel Core i5-12600T, 6 cores and 12 threads |
| Memory | 32 GB DDR5-4800 |
| Integrated graphics | Intel UHD Graphics 770 (Xe-LP), disabled in this EFI with `-wegnoigpu` |
| Discrete graphics | AMD Radeon RX 6600M 8 GB, reported as Radeon Pro W6600 in macOS |
| SSD | SK hynix PC711 1 TB NVMe |
| Wi-Fi and Bluetooth | Broadcom BCM94360NG |
| AirDrop | Supported |
| Ethernet | Two Realtek RTL8125B 2.5GbE ports |
| Onboard audio | Realtek ALC897, layout ID 98 |
| SMBIOS | iMacPro1,1 |
| OS and bootloader | macOS Tahoe 26.7.3 (25G342), OpenCore 1.0.8 MOD |

This EFI uses the RX 6600M for display output and graphics acceleration with WhateverGreen. The WhateverXe project has validated integrated graphics acceleration on the Alder Lake-S UHD 770 and requires WhateverXe and WhateverGreen to be used separately. [WhateverXe documentation](https://github.com/b00t0x/WhateverXe#installation)

## Verified functionality

- Boot, macOS Recovery, rear USB ports, 3.5 mm audio, LAN2 at 2.5 Gb/s, Wi-Fi, and Bluetooth audio pairing.
- The RX 6600M drives one USB-C port, one HDMI port, and both DisplayPort ports.
- SIP and Authenticated Root are enabled. Onboard audio uses SimpleHDA 1.0.1 and the BCM94360NG uses SimpleBCM 1.2.0. [SimpleHDA](https://github.com/laobamac/SimpleHDA/releases/tag/v1.0.1) · [SimpleBCM](https://github.com/laobamac/SimpleBCM/releases/tag/v1.2.0)
- H.264 and HEVC hardware encoding passed basic testing.

## Generate your own SMBIOS identifiers

Copy `EFI/` from the release ZIP or [`V2/EFI`](./V2/EFI) from this repository to the EFI partition on a test USB drive. Edit `EFI/OC/config.plist` on the USB drive and open `PlatformInfo → Generic`. The public EFI has blank `SystemSerialNumber`, `MLB`, and `SystemUUID` fields and six zero bytes in `ROM`. Keep `SystemProductName` set to `iMacPro1,1`.

1. Extract [OpenCore 1.0.8 RELEASE](https://github.com/acidanthera/OpenCorePkg/releases/tag/1.0.8) and run the following command from its `Utilities/macserial` directory. The output places `SystemSerialNumber` before the `|` separator and `MLB` after it. Use both values from the same line and trim the surrounding whitespace.

   ```sh
   ./macserial --model iMacPro1,1 --num 1
   ```

2. Run `uuidgen` in Terminal and copy the result into `SystemUUID`.
3. Run `networksetup -listallhardwareports` to find a physical Ethernet interface and its MAC address. If the chosen interface is `en1`, you can also run `networksetup -getmacaddress en1`. Remove the colons from the MAC address and enter the resulting twelve hexadecimal digits as six bytes in `ROM`. Keep the field type set to `Data`.
4. Save the config on your USB drive and verify that it boots before signing in to your Apple Account. Keep the four identifiers in your local EFI and private backups. Use placeholders in any configuration you publish.

## Changelog

### 2026-11-17 · v1.0.0

- Released the EFI for the liquid-cooled FEVM FN60G-WE with support for macOS Tahoe 26.7.3 (25G342).

### Upstream history

- 2026-07-30
  - Updated to OpenCore 1.0.8 MOD.
  - Added support for installing and running macOS Tahoe.
- 2025-02-25
  - Updated to the OpenCore 1.0.4 development build.
  - Remapped USB ports and fixed audio input, audio output, and the camera on the LG UltraFine 5K.
- 2024-09-22
  - Updated to the OpenCore 1.0.2 development build and IOSkywalkFamily.kext 1.2.0.
  - Added Sequoia support for Broadcom Wi-Fi and Sonoma support for Intel Wi-Fi.
- 2024-03-11
  - Updated to IOSkywalkFamily.kext 1.1.0.
  - For Sonoma 14.4, the upstream upgrade sequence was to update the EFI, install OCLP, and reboot before upgrading to keep Wi-Fi working. [Upstream OCLP instructions](https://pan.daliansky.net/APPS/OCLP/OCLP.md)
- 2024-03-01
  - Updated to OpenCore MOD 0.9.9.
- 2024-01-17
  - Updated to OpenCore MOD 0.9.8.
  - Fixed Hyper-Threading and incorrect CPU model reporting.
