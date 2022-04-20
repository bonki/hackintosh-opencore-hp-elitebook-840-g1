# Hackintosh OpenCore HP EliteBook 840 G1

## Hardware specs

| Component          | Detail                                                                             |
| ------------------ | ---------------------------------------------------------------------------------- |
| Model              | HP EliteBook 840 G1                                                                |
| CPU                | Intel Core i5-4200U @ 1.60GHz                                                      |
| Graphics           | Intel HD Graphics 4400                                                             |
| Display            | 14" LED-backlit HD+ @ 1600x900px                                                   |
| Sound              | IDT 92HD91BXX                                                                      |
| Ethernet           | Intel I218-LM                                                                      |
| Wi-Fi, Bluetooth   | Intel Dual Band Wireless-N 7260AN 802.11 a/b/g/n (2x2) Wi-Fi + Bluetooth 4.0 combo |
| SD card reader     | Realtek RTS5227                                                                    |
| Smartcard reader   | Alcor Micro AU9540                                                                 |
| Webcam             | HP HD Webcam (Chicony Electronics)                                                 |
| Fingerprint reader | Validity Sensors VFS495                                                            |

## Support status

### OS versions

| OS       | Version | Works | Comment  |
| -------- | ------- | :---: | -------- |
| Catalina | 10.15.7 |   x     |          |
| Big Sur  | 11.6.5  |   x     |          |
| Monterey | 12.x.y  |   ?     | Untested |

### Components

| Component          | Works | Comment                                                                                                 |
| ------------------ | :---: | ------------------------------------------------------------------------------------------------------- |
| DisplayPort        |   x   |                                                                                                         |
| VGA                |   x   |                                                                                                         |
| Audio              |   x   | Full support for output switching, internal speakers/microphone, headset mic when using the HP in/out   |
| USB                |   x   |                                                                                                         |
| Trackpad           |   x   | Full multi-finger gesture support                                                                       |
| Trackpoint         |   -   | Probably needs more work on multiplexing support in `VoodooPS2`                                         |
| Special keys       |   x   | Hardware keys for Wi-Fi and audio work, brightness up/down works, volume up/down works                  |
| Sleep/wake         |   x   |                                                                                                         |
| Ethernet           |   x   |                                                                                                         |
| Wi-Fi              |   x   | Requires [HeliPort](https://github.com/OpenIntelWireless/HeliPort) client for administration            |
| AirDrop            |   -   | Unsupported due to the nature of the implementation of the Intel driver. Use Broadcom adapter if needed |
| Bluetooth          |   x   |                                                                                                         |
| Webcam             |   x   |                                                                                                         |
| SD card reader     |   ~   | `Sinetek-rtsx.kext` disabled in `config.plist`. Works but has sleep/wake issues, see repo for more info |
| Smartcard reader   |   -   |                                                                                                         |
| Fingerprint reader |   -   |                                                                                                         |
| Battery            |   x   |                                                                                                         |
| Power Management   |   x   |                                                                                                         |
| CPU temperature monitoring | x |                                                                                                         |
| Docking station    |   ~   | Partially working, for details see below                                                                |

### HP UltraSlim docking station

| Component      | Works |
| -------------- | :---: |
| Power/charging |   x   |
| Power button   |   x   |
| DisplayPort    |   x   |
| VGA            |   x   |
| Audio          |   -   |
| USB            |   -   |
| Ethernet       |   x   |

### BIOS versions

| Version     | Works | Comment          |
| ----------- | :---: | ---------------- |
| 01.30 Rev.A |   x   |                  |
| 01.48 Rev.A |   ?   | Latest, untested |

## Usage

### BIOS settings

- Advanced
  - Boot Options
    - [ ] `Fast Boot`
    - [ ] `Secure Boot`
    - Boot mode: `UEFI Native (Without CSM)`  
        Note: `UEFI Hybrid (With CSM)`works but won't give you native screen resolution during boot process
  - Device Configurations
    - [x] `USB legacy support`
    - [x] `USB 3.0 (XHCI)`
    - Video memory size: `64 MB`
    - [ ] `Wake on USB`
    - [x] `Virtualization Technology (VTx)`
    - Deep Sleep: `Auto`
  - Built-In Device Options
    - Wake on LAN: `Disable`
  - Port Options
    - [ ] Smart Card  
            Note: Optional - lacks support so why keep it enabled

### EFI HFS+ driver

This repo ships with the open source `OpenHfsPlus.efi` EFI HFS+ filesystem driver which is still considered experimental.
It is *only* needed to boot the installation medium (because you should use APFS anyway when installing your system and
OpenCore has builtin support for APFS). In case you are having trouble installing using `OpenHfsPlus.efi` you can use
the proprietary [`HfsPlus.efi`](https://github.com/acidanthera/OcBinaryData/blob/master/Drivers/HfsPlus.efi) (it's
not included here for licensing reasons).
Don't forget to disable `OpenHfsPlus.efi` and enable `HfsPlus.efi` in `config.plist` in this case and make sure to never
have both enabled at the same time.

### PlatformInfo

You will need to generate SMBIOS serial information, otherwise this will not boot.
Use [`GenSMBIOS`](https://github.com/corpnewt/GenSMBIOS) and update `config.plist` as described
[here](https://dortania.github.io/OpenCore-Install-Guide/config.plist/haswell.html#platforminfo).

## Credits

- [`hshamsaldin/OpenCore-HP-840-G1-Haswel`](https://github.com/hshamsaldin/OpenCore-HP-840-G1-Haswel.git)
  for doing all the heavy lifting
- [`OpenCore`](https://github.com/acidanthera/OpenCorePkg)
- [`AppleALC`](https://github.com/acidanthera/AppleALC)
- [`IntelBluetoothFirmware`](https://github.com/OpenIntelWireless/IntelBluetoothFirmware)
- [`IntelMausi`](https://github.com/acidanthera/IntelMausi)
- [`itlwm`](https://github.com/OpenIntelWireless/itlwm)
- [`Lilu`](https://github.com/acidanthera/Lilu)
- [`Sinetek-rtsx`](https://github.com/cholonam/Sinetek-rtsx)
- [`VirtualSMC`](https://github.com/acidanthera/VirtualSMC)
- [`VoodooPS2`](https://github.com/acidanthera/VoodooPS2)
- [`VoodooRMI`](https://github.com/VoodooSMBus/VoodooRMI)
- [`WhateverGreen`](https://github.com/acidanthera/WhateverGreen)

