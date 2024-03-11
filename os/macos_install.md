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

CMD - python macrecovery.py -b Mac-2BD1B31983FE1663 -m 00000000000000000 download

COPY - com.apple.recovery.boot folder to USB
- com.apple.recovery.boot
    - BaseSystem.chunklist
    - BaseSystem.dmg

COPY - EFI(/OpenCore/X64/EFI) folder to USB
```

### EFI Preparing

Gathering miscellaneous files for booting macOS is a nightmare. I now understand why the OpenCore Guide stated that time and patience are crucial prerequisites right from the beginning.


```
/EFI/OC/
- OpenCore.efi
- config.plist
```

```
/EFI/OC/ACPI
```

/EFI/OC/Drivers (Firmware Drivers)
```
- OpenRuntime.efi
Required critical component of the OpenCore boot loader

- OpenCanopy.efi
Official GUI for OpenCore boot loader

- ResetNvramEntry.efi
Allow resetting NVRAM from the boot picker

- HfsPlus.efi
Required for seeing HFS volumes, such as macOS Installers and Recovery partitions. 
https://github.com/acidanthera/OcBinaryData/blob/master/Drivers/HfsPlus.efi

```

/EFI/OC/Kexts (Kernel Extensions)
```
- Lilu.kext
Arbitrary kext and process patching on macOS. 
https://github.com/acidanthera/Lilu/releases

- VirtualSMC.kext
Emulates the SMC chip for Real Mac Devices. 
https://github.com/acidanthera/VirtualSMC/releases

- SMCProcessor.kext
A plugin included with the VirtualSMC release, designed to monitor Intel CPU temperatures.

- SMCSuperIO.kext
A plugin included with the VirtualSMC release, designed to monitor fan speed.

- WhateverGreen.kext
A required kernel extension for graphics patching, all GPUs benefit from this kext.
https://github.com/acidanthera/WhateverGreen/releases

- BlueToolFixup.kext

```

/EFI/OC/Resources
/EFI/OC/Tools