# MacOS on Surface

Installing Hackintosh on a Microsoft Surface is a highly challenging task, and this note isn't intended to serve as a guide for everyone. As a software engineer, I started from scratch and still had to read through approximately 30 pages of various articles to get it done properly.


## References
- OpenCorePkg - https://github.com/acidanthera/opencorepkg/releases
- Dortania's OpenCore Guide - https://dortania.github.io/OpenCore-Install-Guide/
- ChatGPT4 - https://chat.openai.com/
- ProperTree - https://github.com/corpnewt/ProperTree
- olm3ca/Surface-Laptop-Go - https://github.com/olm3ca/Surface-Laptop-Go
- Xiashangning/BigSurface - https://github.com/Xiashangning/BigSurface
- https://www.tonymacx86.com/threads/faq-read-first-laptop-frequent-questions.302852/

## Hardware Specs

```
Computer: Microsoft Corporation Surface Laptop Go (Model 1943)
CPU: Intel Core i5-1035G1 (Ice Lake-U, D1) 1200 MHz (12.00x100.0) @ 3592 MHz (36.00x99.8)
Graphics: Intel Ice Lake-U GT2 32EU LM - Integrated Graphics [Microsoft] Intel UHD Graphics 920, 1024 MB 
Memory: 8192 MBytes @ 1197 MHz, 24-22-22-51
Motherboard: Intel 495 (Ice Lake-U PCH-LP Premium)
BIOS: 17.200.140, 11/02/2023		
UART: Intel(R) Serial IO UART Host Controller 34A8
Sound: Intel Ice Lake-U/Y PCH-LP - cAVS (Audio, Voice, Speech) [D0]
Drive: Hynix HFM256GDGTNG-87A0A, 250.1 GB, NVMe
Network: Intel Wi-Fi 6 AX201 160MHz
```

CATION: YMMV with different hardwares.

## MacOS USB Installer

NOTE: The closest real Mac product that matches my hardware is the Apple MacBook Air 2020 (MacBookAir9,1), which is equipped with the same Intel 10th generation Ice Lake CPU. The pre-installed macOS version on this model is macOS Big Sur (macOS 11). Since then, Apple has transitioned from using Intel CPUs to their own custom-designed Apple Silicon ARM CPUs in the MacBook series. Consequently, macOS versions released after Big Sur are no longer optimized for Intel x86 processors in the same way. Therefore, I personally believe that using Big Sur with the MacBookAir9,1 SMBIOS configuration would be the best fit for my situation.

CATION: YMMV with different macOS versions.

### Download MacOS

Download OpenCorePkg from https://github.com/acidanthera/OpenCorePkg/releases. As of March 10, 2024, the latest version is v0.9.8.
```
OPEN - /OpenCore/Utilities/macrecovery/

READ - recovery_urls.txt for the version information
...
Big Sur 11: -b Mac-2BD1B31983FE1663 -m 00000000000000000
Monterey 12: -b Mac-E43C1C25D4880AD6 -m 00000000000000000
Ventura 13: -b Mac-B4831CEBD52A0C4C -m 00000000000000000
...

CMD - python macrecovery.py -b Mac-E43C1C25D4880AD6 -m 00000000000000000 download

COPY - com.apple.recovery.boot folder to USB
- com.apple.recovery.boot
    - BaseSystem.chunklist
    - BaseSystem.dmg

COPY - EFI(/OpenCore/X64/EFI) folder to USB
```

### EFI Preparing

Gathering miscellaneous files(drivers, patches, configs) for booting macOS is a nightmare. I now understand why the OpenCore Guide stated that time and patience are crucial prerequisites right from the beginning.

/EFI/OC/ACPI (Hardware Confiuration)
```
- SSDT-SURFACE.aml 
Bigsurface prebuilt SSTD for surface peripherals.
VER: 6.5 (BigSurface)
SRC: https://github.com/Xiashangning/BigSurface/releases/

- SSDT-PLUG-DRTNIA.aml
DRTNIA prebuilt SSDT for power management
VER: 3756ef9, 20200528
SRC: https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-PLUG-DRTNIA.aml

- SSDT-EC-USBX-LAPTOP.aml
DRTNIA prebuilt SSDT for fixing Embedded Controller for Skylake laptops and newer.
VER: 3756ef9, 20200528
SRC: https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-EC-USBX-LAPTOP.aml

- SSDT-PNLF.aml
DRTNIA prebuilt SSDT to create a PNLF device for macOS for backlight fix.
VER: e324fea, 20211013
SRC: https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-PNLF.aml

- SSDT-AWAC.aml
DRTNIA prebuilt SSDT to fix the system clocks found on newer hardware(495 series, Icelake).
VER: 3756ef9, 20200528
SRC: https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-AWAC.aml

- SSDT-RHUB.aml
Turn off the RHUB device and force macOS to manually rebuild the ports.
VER: 6f45661, 20200816
SRC: https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-RHUB.aml
```

/EFI/OC/Drivers (Firmware Drivers)
```
- OpenRuntime.efi
Required critical component of the OpenCore boot loader
VER: 0.9.8 (OpenCorePkg)
SRC: https://github.com/acidanthera/OpenCorePkg/releases

- OpenCanopy.efi
Official GUI for OpenCore boot loader
VER: 0.9.8 (OpenCorePkg)
SRC: https://github.com/acidanthera/OpenCorePkg/releases

- ResetNvramEntry.efi
Allow resetting NVRAM from the boot picker
VER: 0.9.8 (OpenCorePkg)
SRC: https://github.com/acidanthera/OpenCorePkg/releases

- HfsPlus.efi
Required for seeing HFS volumes, such as macOS Installers and Recovery partitions. 
VER: c2a9898, 20230104
SRC: https://github.com/acidanthera/OcBinaryData/blob/master/Drivers/HfsPlus.efi
```

/EFI/OC/Kexts (Kernel Extensions)
```
- Lilu.kext
Arbitrary kext and process patching on macOS. 
VER: 1.6.7
SRC: https://github.com/acidanthera/Lilu/releases

- VirtualSMC.kext
Emulates the SMC chip for Real Mac Devices. 
VER: 1.3.2 (VirtualSMC)
SRC: https://github.com/acidanthera/VirtualSMC/releases

- SMCProcessor.kext
A plugin included with the VirtualSMC release, designed to monitor Intel CPU temperatures.
VER: 1.3.2 (VirtualSMC)
SRC: https://github.com/acidanthera/VirtualSMC/releases

- SMCSuperIO.kext
A plugin included with the VirtualSMC release, designed to monitor fan speed.
VER: 1.3.2 (VirtualSMC)
SRC: https://github.com/acidanthera/VirtualSMC/releases

- WhateverGreen.kext
A required kernel extension for graphics patching, all GPUs benefit from this kext.
VER: 1.6.6
SRC: https://github.com/acidanthera/WhateverGreen/releases

- BigSurface.kext
A community project to enpower surface peripherals on MacOS, such as touchpad, keyboard, function buttons, ambient light sensor, battery sensor, touch screen and stylus.
VER: 6.5 (BigSurface)
SRC: https://github.com/Xiashangning/BigSurface/releases/

- AppleALC
AppleHDA patching, allowing support for the majority of on-board sound controllers(v1.7.6 Added ALC298 layout-id 33 for surface laptop 1gen by Rockjesus.cn)
VER: 1.8.9
SRC: https://github.com/acidanthera/AppleALC/releases

- USBToolBox.kext
A tool to customize the configuration of USB ports.
VER: 1.1.1
SRC: https://github.com/USBToolBox/kext/releases

- UTBMap.kext
Mapped USB kext generated by USBToolBox Tool.
VER: 0.2 (USBToolBox Tool)
SRC: https://github.com/USBToolBox/tool/releases

- AirportItlwm.kext
An Intel Wi-Fi Adapter Kernel Extension for macOS.
VER: 2.2.0
SRC: https://github.com/OpenIntelWireless/itlwm/releases

- BlueToolFixup.kext
Patches the macOS 12+ Bluetooth stack to support third-party cards.
VER: 2.6.8 (BrcmPatchRAM)
SRC: https://github.com/acidanthera/BrcmPatchRAM/releases

- IntelBluetoothFirmware.kext
An Intel Bluetooth Firmware to provide native Bluetooth in macOS.
VER: 2.4.0
SRC: https://github.com/OpenIntelWireless/IntelBluetoothFirmware/releases

- IntelBTPatcher.kext
Workaround for a bug in bluetoothd by initializing the bluetooth module properly
VER: 2.4.0
SRC: https://github.com/OpenIntelWireless/IntelBluetoothFirmware/releases

- ECEnabler.kext
Fixes reading battery status on many devices (Allows reading EC fields over 8 bits long)
VER: 1.0.4
SRC: https://github.com/1Revenger1/ECEnabler/releases
```

/EFI/OC/Resources

/EFI/OC/Tools
```
- OpenShell.efi
A shell environment for the UEFI for debugging.
VERSION: 0.9.8 (OpenCorePkg)
SRC: https://github.com/acidanthera/OpenCorePkg/releases
```

### config.plist Setup

- Get a copy of sample.plist from OpenCorePkg and rename to /EFI/OC/config.plist
- Get ProperTree from https://github.com/corpnewt/ProperTree as the xml editor for config.plist
- Open config.plist with ProperTree and perform a "Clean Snapshot" to /EFI/OC/. This will remove all the entries from the config.plist and then adds all your SSDTs, Kexts and Firmware drivers to the config

- Booter->Quirks
```
DevirtualiseMmio FALSE
*Reduces Stolen Memory Footprint

EnableWriteUnprotector FALSE
*This quirk and RebuildAppleMemoryMap can commonly conflict

ProtectUefiServices TRUE
*Protects UEFI services from being overridden by the firmware

RebuildAppleMemoryMap TRUE
*Generates Memory Map compatible with macOS

SetupVirtualMap FALSE
*Fixes SetVirtualAddresses calls to virtual addresses, can cause early kernel panics on Ice Lake machines

SyncRuntimePermissions TRUE
*Fixes alignment with MAT tables and required to boot Windows and Linux with MAT tables
```