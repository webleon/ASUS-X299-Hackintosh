# Thunderbolt

# Introduction
The Thunderbolt folder contains information on how to configure your Thunderbolt 3/4 cards for support in macOS.  While Alpine Ridge cards are supported, currently the recommended card to use is the Gigabyte GC-Titan Ridge V1.0/2.0.  One thunderbolt card is officially supported on most ASUS X299 motherboards but it is possible to use more than one Thunderbolt PCIe card in a system.  However the other cards may not be as stable as the one connected as built-in or configured in the BIOS.

# Thunderbolt 3/4 Connections
Depending on which Thunderbolt card you have, your card may have 1 or 2 6-pin PCIe power connectors.  These are optional to plug in if you want more power to your Thunderbolt devices.  Titan Ridge and Maple Ridge Thunderbolt cards also have a 9 pin USB 2.0 header on the card for USB 2 devices.  If you do not plug USB 2 devices directly into your card, you can ignore plugging this into your motherboard.  

# Jump pins 3 and 5 (Optional)
If you are using an additional thunderbolt card or your motherboard does not have a 5 pin thunderbolt header.  You will need to jump pins 3 and 5 for hot plug capability.

The pins that need to be connected together should look like this.
![](/Thunderbolt/Images/jump3-5.jpeg)

# BIOS Settings
BIOS settings listed below are configured for the first add-in or built-in Thunderbolt card.  If your motherboard does not have a Thunderbolt header you can skip this section.

## Advanced
### Thunderbolt(TM) Configuration
* TBT Root port Selector - PCIe slot Thunderbolt card is in
* Thunderbolt Usb Support - Enabled
* Thunderbolt Boot Support - Enabled
* Wake From Thunderbolt(TM) Devices - On
* Thunderbolt(TM) PCIe Cache-line Size - 128
* GPIO3 Force Pwr - On
* Wait time in ms after applying Force Pwr - 200
* Skip PCI OptionRom - Enabled
* Security Level - SL0-No Security
* Reserve mem per phy slot - 32
* Reserve P mem per phy slot - 32
* Reserve IO per phy slot - 4
* Delay Before SX Exit - 300
* GPIO filter - Enabled
* Enable CLK REQ - Enabled
* Enable ASPM - Enabled
* Enable LTR - Enabled
* Extra Bus Reserved - 106
* Reserved Memory - 737
* Memory Alignment - 26
* Reserved PMemory - 1184
* PMemory Alignment - 28
* Reserve I/O - 0
* Alpine Ridge XHCI WA - Enabled
* AR XHCI Host Pre-wake - Enabled
* AR XHCI Host Active LTR - 20
* AR XHCI Host High LTR - 80
* AR XHCI Host Medium LTR - 40
* AR XHCI Host Low LTR - 30

# OpenCore config.plist Settings
* Uncheck `DisableIoMapper` under `Kernel-Quirks` for AppleVTD support.
  * AppleVTD allows support for devices like Antelope Audio Thunderbolt interfaces or the Apple Thunderbolt-to-Gigabit Ethernet adapter.
* If present, remove `dart=0`, from boot-args located `NVRAM-Add-7C436110-AB2A-4BBB-A880-FE41995C9F82-boot-args`

If configured correctly your IOReg should show that you have AppleVTD and DMAC (Direct Memory Access Controller).
![](/Thunderbolt/Images/applevtd.png)
![](/Thunderbolt/Images/dmac.png)

# Configuration
1. Once BIOS settings and OpenCore settings are configured you should be able to load macOS with initial Thunderbolt support.  If you look in your IOReg the Thunderbolt device tree should look like this.  The location of your Thunderbolt card will vary depending on which slot it is occupied in.  For the WS X299 Sage/10G or any other motherboard set as Built-In it should be at PC00.RP05.  You can verify by searching for 'Thunderbolt' in IOReg.
![](/Thunderbolt/Images/Thunderbolt-PreSSDT.png)
2. For hot-plug support, you will need a Thunderbolt hot-plug SSDT.  Download kgp's SSDT that I modified for OpenCore and more recent BIOS called 'SSDT-TB3HP.aml' and add the SSDT to your EFI folder under `OC-ACPI`.  Also add an entry in your config.plist under `ACPI-Add`.  It is preferred to add this entry at the very bottom since it relies on method DTGP which is located in SSDT-SBUS-MCHC.aml.
3.  Reboot your computer and you should have hot plug support!
  * System Report showing two new devices
  ![](/Thunderbolt/Images/pci-SSDT.png)
  * IOReg showing hot plug SSDT adjustments
  ![](/Thunderbolt/Images/Thunderbolt-AfterSSDT.png)

# Thunderbolt Bus and Local Node Activation
If you have Thunderbolt configured, your Thunderbolt card is running in Standard or ICM (Internal Connection Manager) mode.  Apple developed another mode called Extended which implements more capabilities compared to Windows and Linux.

Switching from Standard to Extended is optional, but it brings your system more in line with a real Mac and provides these additional capabilities:
* Thunderbolt Bus and Local Node
* Thunderbolt Ethernet bridge
* Target Disk mode
* eGPU Support
* Thunderbolt NAS Support
* More support for legacy Thunderbolt 1 devices
* Refer to [this post](https://www.tonymacx86.com/threads/success-gigabyte-designare-z390-thunderbolt-3-i7-9700k-amd-rx-580.267551/post-2095044) for more devices

**[Disclaimer:]** While this brings Thunderbolt capabilities more like a real Mac, it is not perfect.  This affects hot plug support in Windows which may require a warm reboot from Mac then to Windows.  There may also be some inconsistencies with how Thunderbolt devices behave or other side effects.  Generally if your Thunderbolt devices work in ICM mode, **[do not]** proceed with these steps as it involves flashing the Thunderbolt firmware chip located on the Thunderbolt card/motherboard.  **[I assume no responsibility or liability for any damage that may occur so proceed at your own risk!**]

## 1. Guides by CaseySJ for flashing SPI ROM chip
* Mini-Guides
  * [Flashing SPI ROM Chips using Raspberry Pi 3B or 4](https://www.tonymacx86.com/threads/success-gigabyte-designare-z390-thunderbolt-3-i7-9700k-amd-rx-580.267551/post-2080585)
  * [Flashing SPI ROM Chips using 3.3V CH341A Programmer](https://www.tonymacx86.com/threads/success-gigabyte-designare-z390-thunderbolt-3-i7-9700k-amd-rx-580.267551/post-2161463)
* Patched Thunderbolt Firmware files
  * [Repository of Patched Thunderbolt Firmware Files](https://www.tonymacx86.com/threads/success-gigabyte-designare-z390-thunderbolt-3-i7-9700k-amd-rx-580.267551/page-1640#post-2087524)
    * Currently using GC-Titan-Ridge-V2.0-Mod-NVM50-CaseySJ.bin
* Additional Resources
  * [Gigabyte Designare Z390 (Thunderbolt 3) + i7-9700K + AMD RX 580](https://www.tonymacx86.com/threads/success-gigabyte-designare-z390-thunderbolt-3-i7-9700k-amd-rx-580.267551/)

## 2. Configuration
Once you have successfully flashed a custom firmware file following the guide, you should see the Thunderbolt Bus pane populated under `System Report-Thunderbolt/USB4`.
In order to have hot plug support, follow this guide by CaseySJ and Inqnuam [Using HackinDROM to Create Thunderbolt SSDT with Custom DROM](https://www.tonymacx86.com/threads/success-gigabyte-designare-z390-thunderbolt-3-i7-9700k-amd-rx-580.267551/post-2186155).
  * Note that SSDT-DTPG.aml is not needed if you already have SSDT-SBUS-MCHC.aml.

Once you reboot, you should have the Thunderbolt Bus pane properly populated and hot plug support!
  * System Report showing two new devices
  ![](/Personal%20Build/Images/pci.png)
  * System Report showing populated Thunderbolt Bus
  ![](/Thunderbolt/Images/tbbus.png)
  * IOReg showing hot plug SSDT adjustments
  ![](/Thunderbolt/Images/Thunderbolt-tbbus-AfterSSDT.png)

## What Works
* Thunderbolt 3 hot-plug
* USB 2.0 hot-plug/cold-plug
* USB 3.2 Gen 2 cold-plug/warm
* USB 3.2 Gen 2 hot-plug
  * Only if a USB 3.2 Gen 2 device is plugged in at cold boot.  After you can unplug, and replug.  This issue seems to only affect macOS 11.3 or newer
* Sleep

## What Doesn't Work
* USB 3.2 Gen 2 hot-plug
  * Only if there is no device plugged in at boot.
* USB 3.2 Gen 2 cold-plug/warm/hot-plug on BIOS with Resizable Bar Support


# Extras
## Thunderbolt 4
For Thunderbolt 4, there are 3 PCIe Thunderbolt cards available using the Maple Ridge controller.  The information below is based on my testing with the ASUS ThunderboltEX 4.

### Current Observations
Note these observations are based on the devices tested listed below.  YMMV and issues I'm running into may just be due to these devices.
 * Initially ran into issues where card was causing kernel panics in macOS and error code 62 on reboots.  Found this issue on the latest BIOS with Resizable Bar Support.  The thunderbolt card works normally using BIOS 3302.
 * Thunderbolt header may not necessarily be needed or jumped.  macOS recognizes the card on boot without a header plugged in.
 * The 6 pin power and USB 2.0 internal cable have to be connected.  
 * Thunderbolt settings have to be enabled in BIOS.  The Thunderbolt devices load in a different slot with Thunderbolt BIOS settings disabled but the device showed 'No driver installed'.
 * SSDT is required for Thunderbolt devices to be recognized.

### What Works
 * Thunderbolt 3 hot-plug
 * USB 2.0 hot-plug/cold-plug
 * USB 3.2 Gen 2 hot-plug/cold-plug
 * Sleep

### What Doesn't Work
 * Thunderbolt 3 cold/warm boot

### Devices Tested
 * Sabrent Thunderbolt 3 M.2 NVMe SSD Enclosure (EC-T3NS)

### To-Do
 * Extract original firmware and try installing custom firmware for Thunderbolt Local Node support.
 * Try more Thunderbolt 3/4 devices.

# Credits
* kgp
* DSM2
* CaseySJ
* Elias64Fr
* Inqnuam
