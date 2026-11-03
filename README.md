# Lenovo LOQ 15IAX9E (Type 83LK / 83ME) - macOS Tahoe 26.7.x OpenCore EFI
Comprehensive OpenCore EFI configuration for the **Lenovo LOQ 15IAX9E** running **macOS Tahoe (26.7.x)**.
* **Curated, Tested & Patched by:** **Ruwaid Nizam**  
* **OpenCore Version:** `1.0.8` (Validated with 0 errors via `ocvalidate`)  
* **SMBIOS Target:** `MacBookPro16,2`  
---
## Hardware Specifications
| Component | Hardware Specification | macOS Support Status |
| :--- | :--- | :--- |
| **Laptop Model** | Lenovo LOQ 15IAX9E (Type 83LK / 83ME) | Full Support |
| **Processor (CPU)** | Intel Core i5-12450HX (Alder Lake HX, 4P + 4E Cores / 16 Threads) | Working (`-ngreenpm` Turbo Unlocked) |
| **Integrated GPU (iGPU)** | Intel UHD Graphics 16 EU (`8086:468B` spoofed as Tiger Lake `9A49`) | Working (Metal 3 & Quartz Extreme via NootedGreen) |
| **Discrete GPU (dGPU)** | NVIDIA GeForce RTX Laptop GPU | Disabled via `SSDT-Disable_GPU_PEG1.aml` (Saves battery) |
| **Display Panel** | 15.6" FHD (1920x1080) 144Hz IPS (BOE `NE156FHM-NX9`) | Working (Full 144Hz & Backlight control) |
| **Audio Codec** | Realtek ALC257 (`10EC:0257`) on Intel cAVS (`8086:7AD0`) | Working (Native English via AppleALC + SimpleHDA) |
| **Wi-Fi Card** | Realtek RTL8852BE 802.11ax PCIe (`10EC:B852`) | Working (AirportRTW native AirPort menu) |
| **Bluetooth Controller**| Realtek RTL8852BE USB Bluetooth (`0BDA:4853` on Port 14 / `HS05`) | Coexistence Patched / Partial (See details below) |
| **Keyboard & Fn Keys** | Lenovo White Backlit Keyboard + Media / Brightness Hotkeys | Working (VoodooPS2 + BrightnessKeys) |
| **Trackpad** | I2C-HID Multi-touch Precision Trackpad (`MSFT0001` / `ELAN`) | Working (VoodooI2C + VoodooI2CHID, Full Gestures) |
| **Ethernet (LAN)** | Realtek RTL8111 PCI Express Gigabit Ethernet | Working (RealtekRTL8111.kext) |
| **Power Management** | Battery Readout, ACPI Power Wells, Thermal Monitoring | Working (VirtualSMC + SMCBatteryManager + SMCProcessor) |
---
## What Works
- [x] **Full Hardware Graphics Acceleration:** Metal 3, Quartz Extreme, WindowServer compositing, native resolution, and 144Hz refresh rate via `NootedGreen.kext` + `WhateverGreen.kext`.
- [x] **CPU Power Scaling:** Unlocked P-Core turbo headroom via `-ngreenpm` boot-argument and balanced GPU floor via `ngreengtfreq=900`.
- [x] **Native Audio:** Internal Stereo Speakers, 3.5mm combo headphone jack with automatic jack-sensing, and internal dual-array microphone. English device labels verified via Ruwaid's binary patch.
- [x] **Wi-Fi Networking:** Realtek RTL8852BE 802.11ax connects directly to 2.4 GHz and 5 GHz networks via `AirportRTW.kext` without third-party companion tools.
- [x] **Trackpad & Gestures:** Full macOS precision gestures (pinch-to-zoom, two-finger scroll, Mission Control swipe, App Exposé) via `VoodooI2C.kext` + `VoodooI2CHID.kext` (`-vi2c-force-polling`).
- [x] **Keyboard & Hotkeys:** Smooth typing, Fn + F5/F6 Brightness keys, and volume controls via `BrightnessKeys.kext`.
- [x] **Display Backlight:** Native slider control in Control Center and Settings via calibrated `SSDT-PNLF.aml`.
- [x] **USB Architecture:** Precise port mapping via `UTBMap.kext` + `USBToolBox.kext`. All external USB-A and USB-C ports mapped with proper power delivery.
- [x] **Battery & Power States:** Native battery percentage indicator, charging status, cycle count, and thermal states.
- [x] **System Reliability:** Sleep, Wake, Warm Restart, and Shutdown.
---
## Current Known Issues & Hardware Notes
1. **Discrete NVIDIA GPU:**  
   macOS lacks support for modern NVIDIA GPUs. The dGPU is cleanly powered down via `SSDT-Disable_GPU_PEG1.aml` to eliminate power draw and prevent overheating.
2. **Realtek RTL8852BE Bluetooth:**  
   - **Antenna Coexistence Fixed:** The physical RF antenna switch is unblocked using the 1-byte `BTC_MODE_NORMAL` patch.
   - **Firmware Container Stepping:** Newer RTL8852BE steppings (`0BDA:4853`) require Firmware Container Version 3 (`BTNIC003`). Current open-source macOS kexts only support V2 (`RTBTCore`). While BLE advertisement discovery is functional, classic BR/EDR pairing (audio headphones) can be inconsistent depending on the hardware stepping.
   - **Recommended Solutions:**  
     - **Option A (\$5 Fix):** A standard CSR 4.0 or Broadcom BCM20702 USB nano dongle (e.g., TP-Link UB400) works out-of-the-box with `BlueToolFixup`.  
     - **Option B (Card Swap):** Replace the internal M.2 card with an **Intel AX210 / AX200** (supported natively by OpenIntelWireless for Wi-Fi 6 + Bluetooth 5.2).
---
## Special Patches & Credits
