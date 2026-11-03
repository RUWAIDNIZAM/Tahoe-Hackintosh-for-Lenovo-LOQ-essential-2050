## Special Patches & Credits
### 1. AirportRTW 1-Byte Coexistence Patch (`BTC_MODE_NORMAL`)
* **Credits:** Community & Discord contributors for identifying the Realtek antenna lock (`BTC_MODE_WL` $\to$ `BTC_MODE_NORMAL`).
* **Implementation by Ruwaid Nizam:** Reverse-engineered the exact binary offset `0x2e7d` in the latest `AirportRTW` build and aligned the OpenCore kernel patch:
  * **Find:** `be01000000e849d6ffff` (`mov esi, 1` - Wi-Fi only)
  * **Replace:** `be00000000e849d6ffff` (`mov esi, 0` - Normal shared coexistence)
### 2. SimpleHDA English Localization Patch
* **Patched by Ruwaid Nizam:** Directly binary-patched `SimpleHDA` at offset `0x16FD0` (`zh_CN\0` $\to$ `en_US\0`), eliminating Chinese fallback strings and delivering clean English device labels ("Internal Speakers", "Headphones", "Internal Microphone").
### 3. Core Software & Driver Acknowledgments
* **Acidanthera:** OpenCorePkg, Lilu, WhateverGreen, AppleALC, VirtualSMC, BlueToolFixup, RestrictEvents, BrcmPatchRAM.
* **ChefKissInc / NootedGreen Team:** NootedGreen graphics driver for 11th/12th Gen Intel Iris Xe / UHD spoofs.
* **Senmiko & FranciumOS:** AirportRTW Wi-Fi driver development.
* **VoodooI2C Team:** VoodooI2C and VoodooPS2 controller implementations.
---
## Post-Installation: Generating Your Own Serial Numbers
> [!IMPORTANT]
> The included `config.plist` has been sanitized for security (`MLB`, `SystemSerialNumber`, `SystemUUID`, and `ROM` have been cleared with placeholders). You **MUST** generate your own serial numbers before logging into Apple ID or iCloud.
1. Download **[GenSMBIOS](https://github.com/corpnewt/GenSMBIOS)**.
2. Run `GenSMBIOS.bat` (Windows) or `GenSMBIOS.command` (macOS).
3. Select option **1** to download MaciASL / latest database.
4. Select option **3** (Generate SMBIOS).
5. Type: `MacBookPro16,2` and press Enter.
6. Open your `EFI/OC/config.plist` using ProperTree or a text editor and copy the generated values into `PlatformInfo -> Generic`:
   - `SystemSerialNumber`
   - `MLB` (Board Serial)
   - `SystemUUID` (SmUUID)
   - `ROM` (Can be set to your machine's Ethernet/Wi-Fi MAC address without colons)
7. Save `config.plist`, reboot, and enjoy macOS Tahoe!
