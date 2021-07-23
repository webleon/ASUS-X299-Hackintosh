# ASUS X299 Hackintosh

# Introduction
The ASUS X299 Hackintosh repository contains OpenCore EFI distributions and related files that can be used as a reference when setting up or updating your X299 Hackintosh with the OpenCore bootloader.  While the EFIs can be used as a starting point for all ASUS X299 boards, it is still highly recommended to review the [OpenCore Vanilla Desktop Guide](https://dortania.github.io/OpenCore-Install-Guide/) and [Skylake-X section](https://dortania.github.io/OpenCore-Install-Guide/config-HEDT/skylake-x.html) for a full guide.  These files were created using the ASUS WS X299 Sage/10G as a baseline.

# Repository Folders
| Folder | Description |
| :------------- | :---------- |
| BASE-EFI | Pre-built OpenCore EFI distributions that should be valid for all ASUS X299 boards |
| Custom BIOS Collection | Modified BIOS .CAP files that have custom boot logos and/or other modifications |
| Personal Build | Information about personal build using the ASUS WS X299 Sage/10G running macOS Monterey |
| Thunderbolt | Information about configuring Thunderbolt 3/4 for macOS |
| USB Kexts | Mapped motherboard USB kexts |

# Optional USB Peripherals for Internal USB 2.0 Devices
* 1. [USB 3.0 20 Pin Female to USB 2.0 Pin Male Adapter](https://www.amazon.com/gp/product/B01MFB04JP/ref=ppx_yo_dt_b_search_asin_title?ie=UTF8&psc=1)
    * This adapter converts the internal USB 3.0 19 pin header to a USB 2.0 9 pin.
* 2. [USB 2.0 9 Pin Header 1 to 4 Extension Hub Splitter](https://www.amazon.com/gp/product/B085KVH16T/ref=ppx_yo_dt_b_search_asin_title?ie=UTF8&psc=1)
* 3. [USB 2.0 IDC 5 Male to USB A Male adapter](https://www.amazon.com/gp/product/B000V6WD8A/ref=ppx_yo_dt_b_search_asin_title?ie=UTF8&psc=1)
    * Uses one of the USB ports on the back of the motherboard to connect internal devices in the case.  You can use two of these together and remove 1 of the pins to make a "9 pin internal adapter".  If the cable is too short to reach inside the case, you can extend with a USB A extension cable.
* 4. PCIe USB 3.0 Card with Internal USB Connector
    * [Inateck USB 3.0 Card](https://www.amazon.com/Inateck-Express-Controller-Internal-Connector/dp/B00JFR2H64/ref=sr_1_3?dchild=1&keywords=inateck+pcie+card&qid=1592455853&s=electronics&sr=1-3)
    * [StarTech USB 3.1 PCIe Card](https://www.amazon.com/gp/product/B01I39D15A/ref=ppx_yo_dt_b_search_asin_title?ie=UTF8&psc=1)
    * Can use the internal header on the card for the case USB ports or to connect internal devices.
    * **NOTE**: Wake from Bluetooth devices does not work with this so it's best to connect Bluetooth to one of the motherboard ports.

# Intel 10 Gigabit NICs with Small Tree macOS Drivers
Intel 10 gigabit NICs such as the X540 or X550 (found on the WS X299 Sage/10G) do not have official support in macOS.  However with modding the EEPROM, we can modify the Subsystem ID to 000a to be compatible with the SmallTree macOS Drivers.  Original method by Squuiid outlined in this [MacRumors thread](https://forums.macrumors.com/threads/modify-retail-intel-10gbe-nics-to-use-small-tree-macos-drivers.1968456/).  

**Disclaimer: The instructions below are outlined for the X550-AT2 found on the WS X299 Sage/10G.  Other Intel NICs may have different commands and/or offsets.  I assume no responsibility or liability for any damage that may occur so proceed at your own risk!**

**Before EEPROM modding: No Driver Installed**
![](/Resources/Images/eeprombefore.png)

**After EEPROM modding and SmallTree drivers: Driver Installed**
![](/Resources/Images/eepromafter.png)

## Requirements
1. [Ubuntu 16.04 LTS](https://releases.ubuntu.com/16.04/)
    * Download the 64-bit PC (AMD64) desktop image
    * Newer versions of Ubuntu do not work.
2. USB Flash drive 8GB+
3. Windows install for installing Ubuntu to USB drive using [Rufus](https://rufus.ie)
4. Ethernet Drivers (SmallTree is recommended but Sonnet also works)
    * [SmallTree macOS drivers](https://small-tree.com/support/downloads/10-gigabit-ethernet-driver-download-page/)
        * Download the macOS Catalina drivers for macOS Big Sur and macOS Monterey
    * [Sonnet macOS drivers](http://www.sonnettech.com/support/kb/kb.php?cat=513&expand=&action=a3#a3)

## Instructions
1. Install the Ubuntu 16.04 LTS ISO image to a flash drive using Rufus.
2. Boot Ubuntu and click "Try Ubuntu without Installing".
    * Once Ubuntu is loaded, make sure that you have an Internet connection.
3. Open Terminal and enter in the following commands to install "net-tools" and "ethtool"
    * `sudo apt-get install net-tools`
    * `sudo apt-get install ethtool`
4. To locate the address of your ethernet devices, enter in command: `ifconfig`.  In my case my X550 ports were `enp180s0f0` and `enp180s0f1`.
![](/Resources/Images/ifconfig.png)
5. To locate the current Vendor/Device-ID of your ethernet devices, enter in command `lspci -nn -vvv | grep Ethernet`.
![](/Resources/Images/lspcigrep.png)
6. Using command `sudo ethtool -e enp180s0f0 | less` , we can search for the offset of the Subsytem Vendor/Subsystem ID (X550 is offset 0x240 - 0x243).
![](/Resources/Images/beforeoffset.png)
7. For SmallTree driver support, we will modify the Subsytem-ID from `8712` to `000a`.  
For the X550-AT2, the commands are:
    * `sudo ethtool -E enp180s0f0 magic 0x15638086 offset 0x242 value 0x0a`
    *  `sudo ethtool -E enp180s0f0 magic 0x15638086 offset 0x243 value 0x00`

    * `sudo ethtool -E enp180s0f1 magic 0x15638086 offset 0x242 value 0x0a`
    * `sudo ethtool -E enp180s0f1 magic 0x15638086 offset 0x243 value 0x00`
![](/Resources/Images/afteroffset.png)
8. For Sonnet driver support, we will modify the Subsystem-Vendor-ID from `1043` to `16b8` and the Subsystem ID from `8712` to `7212`.  
For the X550-AT2, the commands are:
    * `sudo ethtool -E enp26s0f0 magic 0x15638086 offset 0x240 value 0xb8`
    * `sudo ethtool -E enp26s0f0 magic 0x15638086 offset 0x241 value 0x16`
    * `sudo ethtool -E enp26s0f0 magic 0x15638086 offset 0x242 value 0x12`
    * `sudo ethtool -E enp26s0f0 magic 0x15638086 offset 0x243 value 0x72`

    * `sudo ethtool -E enp26s0f1 magic 0x15638086 offset 0x240 value 0xb8`
    * `sudo ethtool -E enp26s0f1 magic 0x15638086 offset 0x241 value 0x16`
    * `sudo ethtool -E enp26s0f1 magic 0x15638086 offset 0x242 value 0x12`
    * `sudo ethtool -E enp26s0f1 magic 0x15638086 offset 0x243 value 0x72`
    * Note if you want to revert back to SmallTree or Intel drivers, we will modify the Subsystem-Vendor-ID back to `1043`.  For the X550-AT2, the commands are:
        * `sudo ethtool -E enp180s0f0 magic 0x15638086 offset 0x240 value 0x43`
        * `sudo ethtool -E enp180s0f0 magic 0x15638086 offset 0x241 value 0x10`

        * `sudo ethtool -E enp180s0f1 magic 0x15638086 offset 0x240 value 0x43`
        * `sudo ethtool -E enp180s0f1 magic 0x15638086 offset 0x241 value 0x10`
9.  Reboot back into macOS and install the appropriate drivers and your ethernet ports should be enabled!

# Additional Kexts
* [SmallTreeIntel82576](https://github.com/khronokernel/SmallTree-I211-AT-patch/releases)
  * Enables ethernet for I211 NICs
  * Version 1.3 is for macOS Catalina, Version 1.2.5 is for macOS 10.13 and 10.14
* [AGPMInjector](https://github.com/Pavo-IM/AGPMInjector)
  * Apple Graphics Power Management injector

# Credits
* [Apple](https://www.apple.com) : macOS
* [Acidanthera](https://github.com/acidanthera) : OpencorePkg, kexts, etc.
* [Dortania](https://github.com/dortania) : Opencore guide
* [Pavo](https://github.com/Pavo-IM) : AGPMInjector
