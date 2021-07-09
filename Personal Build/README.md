# Personal Build
ASUS WS X299 Sage/10G

![](/Personal%20Build/Images/WSX299Sage10G.png)

## Specifications
### Components

| Component        | Model                                | Notes |
| ---------------- | ---------------------------------------|-------------------|
| Motherboard | ASUS WS X299 Sage/10G | BIOS 3302 - Thunderbolt USB 3.0 ports do not work on 3405 with flashed card
| Processor | Intel i9-10980XE | |
| CPU Cooler | Fractal Design Celsius+ S36 Dynamic | |
| RAM | 4x16 Corsair Vengeance LPX 3200 Mhz | |
| Boot Drive | Sabrent Rocket 1 TB | |
| Graphics Card | AMD Radeon Pro W5500 | |
| Wifi/Bluetooth Card | Broadcom BCM943602CDP |  |
| Power Supply | Corsair RM 850x | |
| Case | Lian Li PC 011 Dynamic | |

### PCIe Slot Layout
| Slot | Speed | Device | Notes |
| ----- | ----- | ---------------------------------------|-------------------|
| 1 | x16 | AMD Radeon Pro W5500 | |
| 2 | x8 | | |
| 3 | x8 | | |
| 4 | x8 | | |
| 5 | x8 | Gigabyte GC-Titan Ridge V2.0 | |
| 6 | x8 | | |
| 7 | x8 | Broadcom BCM943602CDP | |

### M.2/U.2 Layout
| Slot | Device | Notes |
| ----- | ---------------------------------------|-------------------|
| U.2_1 | | |
| M.2_1 | Samsung 970 EVO 1 TB | |
| M.2_2 | Sabrent Rocket 1 TB | |

## What Works / What Doesn't Work
- [x] Sleep / Wake
- [x] Wifi and Bluetooth
- [x] Handoff, Continuity, AirDrop, Continuity Camera, and Unlock with Apple Watch
- [x] iMessage, FaceTime, App Store, iTunes Store
- [x] 10G Ethernet
    * Modified EEPROM to work with SmallTree drivers. Refer to section [Intel 10 Gigabit NICs with Small Tree macOS Drivers](https://github.com/shinoki7/ASUS-X299-Hackintosh#intel-10-gigabit-nics-with-small-tree-macos-drivers) for more info.
- [x] HEVC, H.264
- [x] Onboard audio
- [x] TRIM
- [x] USB 2.0
- [x] USB 3.2 Gen 1
- [x] USB 3.2 Gen 2
- [x] DRM
- [x] Native NVRAM
- [x] CPU Power Management
- [x] USB Power
- [x] Thunderbolt 3 hot-plug
- [ ] SideCar
    * Due to some T2 chip dependancies on MacPro7,1 and iMacPro1,1 SMBIOS

## Radeon Pro W5500 Issues
While the Radeon Pro W5500 is supported OOB, I have been running into two major issues.  The major issue appears to related to AGPM (Apple Graphics Power Management).  I have heard success stories from other users with the W5500 but none on the X299 platform.  I'm not sure if these issues are due to a misconfiguration and/or the X299 platform or just specific to my GPU.

### 1. Kernel Panics with Green/Black screen
macOS will randomly kernel panic and freeze (sometimes once a day, sometimes multiple) with a green screen (HDMI) or black screen (DP).  The computer will either stay on a green/black screen until manually rebooting or macOS will reboot automatically with a kp log.

Things I've tried:
* Various versions of macOS Big Sur
* Different SMBIOS (iMacPro1,1, MacPro7,1)
* Different Ram/XMP settings
* Enable/Disable Whatevergreen + boot-arg agdpmod=pikera
* Enable/Disable AGPMInjector.kext

### 2. Hit or miss DP/HDMI audio
DP/HDMI audio sometimes does not work and audio selection is missing from macOS Sound Preferences on consecutive reboots.  Not sure if this is due to my DP to HDMI adapters as I haven't tried testing with just DP cables attached.

### Workarounds
After some troubleshooting, I found that keeping a video open in the background seems to prevent kernel panics.  The video does not have to be continuously playing so I just had a small video clip set to open on login.

The better workaround I found was to adjust the Device Properties to device spoof to a 5500XT.

| Key | Class | Value |
| :--- | :--- | :--- |
| compatible | String | pci1002,7341 |
| device-id | Data | 40730000 |
| agdpmod | String | Pikera |

Also added the Radeon boost SSDT located [here](https://www.tonymacx86.com/threads/amd-radeon-performance-enhanced-ssdt.296555/).  With device spoofing, DP/HDMI audio is more stable as well although the monitor/tv has to be on at boot for audio to work (at least with DP to HDMI adapters, haven't tested DP to DP connections).  This isn't really an issue for me since I only have audio connected to my main monitor.

With the combination of the Radeon boost SSDT and Device Properties adjustments, the amount of kps have reduced significantly and I rarely run into the issues anymore.  Newer versions of macOS Big Sur and Monterey also appear to be more stable.

## Screenshots
### System Report
![](/Personal%20Build/Images/aboutthismac.png)
![](/Personal%20Build/Images/overview.png)
### Memory
![](/Personal%20Build/Images/memory1.png)
![](/Personal%20Build/Images/memory2.png)
### Audio
![](/Personal%20Build/Images/audio.png)
### Ethernet
![](/Personal%20Build/Images/ethernet.png)
### GPU
![](/Personal%20Build/Images/graphics.png)
### NVME
![](/Personal%20Build/Images/nvme.png)
### USB
![](/Personal%20Build/Images/usb.png)
### PCI
![](/Personal%20Build/Images/pci.png)
### Thunderbolt
![](/Personal%20Build/Images/tbbus.png)
