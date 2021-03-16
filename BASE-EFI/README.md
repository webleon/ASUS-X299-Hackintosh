# BASE-EFI

# Introduction
The Base EFI folder contains a precompiled EFI that should be valid for all ASUS X299 motherboards.  It is currently built using OpenCore 0.6.6 with the OpenCanary GUI enabled following the Dortania OpenCore Vanilla Guide.

# 1. Recommended BIOS Settings
* Based off kgp's original X299 Clover guide [section B1) ASUS BIOS Configuration](https://www.tonymacx86.com/threads/imac-pro-x299-live-the-future-now-with-macos-10-14-mojave-successful-build-extended-guide.255082/)
* Reset to Default Settings before changing these settings

## AI Tweaker
* AI Overclock Tuner - Enabled

## Advanced
### CPU Configuration
* MSR Lock Control - **[Disabled]**
    
    <details>
    <summary>CPU Power Management Configuration</summary>
    <p>

    * Enhanced Intel SpeedStep Technology - Enabled
    * Turbo Mode - Enabled
    * Autonomous Core C-State - Enabled
    * Enhanced Halt State (C1E) - Enabled
    * CPU C6 Report - Enabled
    * Package C State - C6(non Retention) state
    * Intel(R) Speed Shift Technology - Enabled
    * MFC Mode Override - OS Native Support
    
    </p>
    </details>
    
### System Agent (SA) Configuration
* Intel VT for Directed I/O (VT-d) - Enabled
### PCH Storage Configuration
* SATA Mode Selection - AHCI

## Boot
* Above 4G Decoding - **[On]**
### CSM (Compatability Support Module)
* Launch CSM - **[Disabled]**
### Secure Boot
* OS Type - Other OS

# 2. Configuration
1. Ethernet: 
    * For WS X299 Sage/10G users replace IntelMausi with [SmallTreeIntel8259x](https://small-tree.com/support/downloads/10-gigabit-ethernet-driver-download-page/) kext and update the kext entry.  NOTE: Ubuntu EEPROM modding outlined [here](https://github.com/shinoki7/ASUS-X299-Hackintosh#intel-10-gigabit-nics-with-small-tree-macos-drivers) is required for this kext to work
    * For users with I211 NICs like the X299 Deluxe, copy the [SmallTreeIntel82576](https://github.com/khronokernel/SmallTree-I211-AT-patch/releases) kext to your EFI folder and add a new kext entry under `Kernel-Add`
2. PlatformInfo: 
    The Base EFI contains two config.plist depending on which SMBIOS you choose.  Rename the SMBIOS you prefer to 'config.plist' and delete the other one.  
    You will need to create your own Serial Number and SMUUID.  Instructions can be found [here](https://dortania.github.io/OpenCore-Install-Guide/config-HEDT/skylake-x.html#platforminfo)
    * Remember to adjust the Type depending on which SMBIOS you are using.  Either iMacPro1,1 or MacPro7,1
    * NOTE: MacPro7,1 only works on Catalina and higher.
    * Using your results from GenSMBIOS, adjust the following (replace '[Removed]')
        * `PlatformInfo-Generic`
            * MLB: Board Serial
            * SystemSerialNumber: Serial
            * SystemUUID: SmUUID
3. USB:
    The Base-EFI still has `XhciPortLimit` enabled with `USBInjectAll.kext`.  However with release of macOS 11.3 this will no longer be supported.  If you have not created your own usb kext, please install a version of macOS lower than 11.3 to create your own mapping.
    * Please use [this](https://dortania.github.io/OpenCore-Post-Install/usb/intel-mapping/intel.html) as a proper guide to map your USB ports.
    * Once mapped make sure to replace the `USBInjectAll.kext` entry under `Kernel-Add` with `USBMap.kext`.  Also disable `XhciPortLimit` under `Kernel-Quirks`.
    
# Changelog:
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


