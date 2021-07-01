# Thunderbolt

# Introduction
The Thunderbolt folder contains information on how to setup your thunderbolt 3 cards for support in macOS.  While Alpine Ridge cards are supported, currently the recommended card to use is the Gigabyte GC-Titan Ridge V1.0/2.0.  One thunderbolt card is officially supported on most ASUS X299 motherboards but it is possible to use more than one Thunderbolt PCIe card.  However the other cards may not be as stable as the one connected as built-in or configured in the BIOS.

# Requirements
* Thunderbolt 3 PCIe card or built-in.
* Thunderbolt hot-plug SSDT.
* Thunderbolt 5-pin Header (If using a PCIe card).
  * If your motherboard does not have a Thunderbolt header, you can jump pins 3 and 5.  Refer to screenshot ...

# BIOS Settings
BIOS settings listed below are configured for the first add-in or built-in Thunderbolt card.  If your motherboard does not have a Thunderbolt header you can skip this section.

# OpenCore config.plist Settings
* Uncheck `DisableIoMapper` under `Kernel-Quirks` for AppleVTD support.
  * AppleVTD allows support for Antelope Audio Thunderbolt interfaces or the Apple Thunderbolt-toGigabit Ethernet adapter to work.
* Remove `dart=0`, if present from boot-args located `NVRAM-Add-7C436110-AB2A-4BBB-A880-FE41995C9F82-boot-args`

# Configuration

# Screenshots

# Extras
## Thunderbolt 4
For Thunderbolt 4, there are 3 PCIe Thunderbolt cards available using the Maple Ridge controller.  The information below is based on my testing with the ASUS ThunderboltEX 4.

### Current Observations
Note these observations are based on the devices tested listed below.  YMMV and issues I'm running into may just be due to these devices.
 * Initially ran into issues where card was causing kernel panics in macOS and error code 62 on reboots.  Found this issue on the latest BIOS with Resizable Bar Support.  Downgraded BIOS to 3302 and the card works normally.
 * Thunderbolt header may not necessarily be needed or jumped.  macOS recognizes the card on boot without a header plugged in.
 * The 6 pin power and USB 2.0 internal cable have to be connected.  
 * Thunderbolt settings have to be enabled in BIOS.  The Thunderbolt devices load in a different slot with Thunderbolt BIOS settings disabled but the device showed 'No driver installed'.
 * SSDT is required for Thunderbolt devices to be recognized.

### What Works
 * Thunderbolt 3 hot-plug
 * USB 2.0 hot-plug/cold-plug
 * USB 3.1 Gen 2 hot-plug/cold-plug
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
