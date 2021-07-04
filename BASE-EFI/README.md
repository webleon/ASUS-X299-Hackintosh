# BASE-EFI

# Introduction
The Base EFI folder contains a pre-built EFI that should be valid for all ASUS X299 motherboards.  It is built using my personal build as a baseline and is up to date using OpenCore 0.7.1 with the OpenCanary GUI following the Dortania OpenCore Vanilla Guide.

# 1. Recommended BIOS Settings
* Originally based off kgp's original X299 Clover guide [section B1) ASUS BIOS Configuration](https://www.tonymacx86.com/threads/imac-pro-x299-live-the-future-now-with-macos-10-14-mojave-successful-build-extended-guide.255082/) but modified for the newest BIOS revisions.  It's recommended to use a newer BIOS release.
* Reset to Default Settings before modifying these settings

## AI Tweaker
* AI Overclock Tuner - XMP
* CPU SVID Support - Enabled

## Advanced
### CPU Configuration
* MSR Lock Control - **[Disabled]**
#### CPU Power Management Configuration
* Enhanced Intel SpeedStep Technology - Enabled
* Turbo Mode - Enabled
* Autonomous Core C-State - Enabled
* Enhanced Halt State (C1E) - Enabled
* CPU C6 Report - Enabled
* Package C State - C6(non Retention) state
* Intel(R) Speed Shift Technology - Enabled
* MFC Mode Override - OS Native Support

### System Agent (SA) Configuration
* Intel VT for Directed I/O (VT-d) - Enabled
### PCI Subsystem Settings
* Above 4G Decoding - **[Enabled]**
* Re-Size BAR Support - **[Disabled]**
### PCH Storage Configuration
* SATA Mode Selection - AHCI

## Boot
### CSM (Compatability Support Module)
* Launch CSM - **[Disabled]**
### Secure Boot
* OS Type - Other OS

# 2. Configuration
1. Ethernet:
    * WS X299 Sage/10G users will need to replace IntelMausi with [SmallTreeIntel8259x](https://small-tree.com/support/downloads/10-gigabit-ethernet-driver-download-page/) kext and update the kext entry.  NOTE: Ubuntu EEPROM modding outlined [here](https://github.com/shinoki7/ASUS-X299-Hackintosh#intel-10-gigabit-nics-with-small-tree-macos-drivers) is required for this kext to work
    * I211 NICs users like the X299 Deluxe, copy the [SmallTreeIntel82576](https://github.com/khronokernel/SmallTree-I211-AT-patch/releases) kext to your EFI folder and add a new kext entry under `Kernel-Add`
2. TSCAdjustReset:
    * Replace TSCAdjustReset.kext in your EFI Folder with the version matching your core count located [here](https://github.com/shinoki7/ASUS-X299-Hackintosh/tree/main/BASE-EFI/Kexts/TSCAdjustReset).
3. PlatformInfo:
    The Base EFI contains two config.plist (iMacPro1,1 and MacPro7,1).  Rename the config.plist file with the SMBIOS you prefer to 'config.plist' and delete the other one.  
    You will need to create your own Serial Number and SMUUID.  Instructions can be found [here](https://dortania.github.io/OpenCore-Install-Guide/config-HEDT/skylake-x.html#platforminfo).
    * Remember to adjust the Type depending on which SMBIOS you are using.  Either iMacPro1,1 or MacPro7,1
    * NOTE: MacPro7,1 only works on Catalina and higher.
    * Using your results from GenSMBIOS, adjust the following (replace '[Removed]')
        * `PlatformInfo-Generic`
            * MLB: Board Serial
            * SystemSerialNumber: Serial
            * SystemUUID: SmUUID

# 3. Post-Install
1. USB:
    * Please use [this](https://dortania.github.io/OpenCore-Post-Install/usb/intel-mapping/intel.html) as a proper guide to map your USB ports.
    * Once mapped, make sure to replace the `USBInjectAll.kext` entry under `Kernel-Add` with `USBMap.kext`.  Also disable `XhciPortLimit` under `Kernel-Quirks`.
    * NOTE: The `XhciPortLimit` quirk may not work on macOS 11.3+ so it's recommended to install an older version to map and then update.
2. Custom Memory (SMBIOS MacPro7,1 only):
    * Depending on how many memory DIMM slots on your motherboard are filled, rename the Memory Dictionary under `PlatformInfo` and remove the other one.  (I.E. I only have 4 DIMM slots filled, so I renamed `#Memory - 4 DIMMS` to `Memory` and deleted `#Memory - 8 DIMMS` and `#Memory - 2 DIMMS`).
    * Expand `Devices` under `PlatformInfo-Memory-Devices` and adjust the following properties that match your Memory.
      * Manufacturer
      * Part Number
      * Serial Number
      * Size
      * Speed

    For 2 DIMMS, this would be BANK 5/3.

    For 4 DIMMS, this would be BANK 7/9/6/4.

    For 8 DIMMS, this would be BANK 7/8/9/10/6/5/4/3.
    * Once mapped make sure to remove `RestrictEvents.kext` under `Kernel-Add` and also delete the kext in your `Kexts` folder under `OC-Kexts`.

# Extras
## macOS Monterey Installation Notes
### Required
* Replace CpuTscSync.kext with TSCAdjustReset.kexts
* OpenCore EFI compatible with macOS Big Sur although recommended to upgrade to OpenCore 0.7.1 or newer and associated Lilu kexts for the latest support.

### Tips
* macOS Monterey update may get stuck in a reboot loop and then fail to install. Modify your config.plist and change `SecureBootModel` to 'Disabled' under `Misc-Security`. When the update is complete, you can change this back to 'Default'.

# Changelog:
## OpenCore 0.7.0 (2021.06.07)
Bootloader / Kexts:
* NVMeFix 1.0.8
* AppleALC 1.6.1
* VirtualSMC 1.2.4
* RestrictEvents 1.0.2
* WhateverGreen 1.5.0
* Updated "Resources" files for OpenCanopy GUI

config.plist Changes:
* Adjusted PickerAttributes to `17` and PickerVariant to `Acidanthera\GoldenGate`

## OpenCore 0.6.9 (2021.05.03)
Bootloader / Kexts:
* Lilu 1.5.3
* NVMeFix 1.0.7
* AppleALC 1.6.0
* VirtualSMC 1.2.3
* IntelMausi 1.0.6
* RestrictEvents 1.0.1

config.plist Changes:
* Added Custom Memory examples for SMBIOS MacPro7,1.  For more info, refer to [post](https://www.tonymacx86.com/threads/gigabyte-b550-vision-d-thunderbolt-3-amd-ryzen-7-3700x-amd-rx-5600-xt.304553/post-2246642).

## OpenCore 0.6.8 (2021.04.05)
Bootloader / Kexts:
* Lilu 1.5.2
* NVMeFix 1.0.6
* WhateverGreen 1.4.9
* AppleALC 1.5.9
* VirtualSMC 1.2.2
* Updated "Resources" files for OpenCanopy GUI

## Updated required SSDTs (2021.03.19)
* Replaced base SSDTs to the ones from the Dortania Guide

## Recommended BIOS Settings (2021.03.11)
* Moved BIOS Settings back to BASE-EFI README.

## OpenCore 0.6.7 (2021.03.01)
Bootloader / Kexts:
* WhateverGreen 1.4.8
* AppleALC 1.5.8
* VirtualSMC 1.2.1

## OpenCore 0.6.6 (2021.02.04)
Bootloader / Kexts:
* Lilu 1.5.1
* WhateverGreen 1.4.7
* AppleALC 1.5.7
* VirtualSMC 1.2.0
* Updated "Resources" files for OpenCanopy GUI

config.plist Changes:
* Misc -> Boot -> PickerAttributes - 25 (Mouse control in Picker)
* Misc -> Boot -> BootProtect -> OC 0.6.6 replaces with LauncherOption and is defaulted to Full
If you were previously using Bootstrap refer to this [link](https://dortania.github.io/OpenCore-Post-Install/multiboot/bootstrap.html#prerequisites/) for instructions on how to upgrade. My previous EFis had it disabled.
* UEFI -> Drivers -> Added OpenHfsPlus.efi (Note: Commented out)
Note that OpenHfsPlus.efi is now bundled with OpenCore but HfsPlus.efi is still enabled for compatibility reasons and is still faster than OpenHfsPlus.efi.
