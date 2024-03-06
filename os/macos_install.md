# MacOS on Surface

## References
- ChatGPT4 - https://chat.openai.com/
- OpenCorePkg - https://github.com/acidanthera/opencorepkg/releases
- Dortania's OpenCore Guide - https://dortania.github.io/OpenCore-Install-Guide/
- ProperTree - https://github.com/corpnewt/ProperTree
- olm3ca/Surface-Laptop-Go - https://github.com/olm3ca/Surface-Laptop-Go
- Xiashangning/BigSurface - https://github.com/Xiashangning/BigSurface



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

"Download gibMacOS from https://github.com/corpnewt/gibMacOS. This Python script allows you to directly download macOS components from Apple. I'll assume that you have the Python environment properly set up on your Windows system. Simply run gibMacOS.bat as an administrator and select your preferred version. In my case, the macOS Big Sur 11.7.10 (20G1427):

```
BuildManifest.plist
Info.plist
InstallAssistant.pkg
InstallInfo.plist
MajorOSInfo.pkg
UpdateBrain.zip
```