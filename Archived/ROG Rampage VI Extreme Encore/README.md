# ROG Rampage VI Extreme Encore

![](/Archived/ROG%20Rampage%20VI%20Extreme%20Encore/Images/RampageVIExtremeEncore.png)

## Specifications
### Components
| Component        | Model                                | Notes |
| ---------------- | ---------------------------------------|-------------------|
| Motherboard | ASUS ROG Rampage VI Extreme Encore | BIOS 0901 |
| Processor | Intel i9-10980XE | |
| CPU Cooler | Corsair iCue H150i Elite Capellix | |
| RAM | 4x16 Corsair Dominator Platinum RGB 3200 Mhz | |
| Boot Drive | Samsung 970 EVO 1 TB | |
| Graphics Card | Sapphire RX 580 Pulse 8 GB | |
| Wifi/Bluetooth Card | Broadcom BCM94360NG | Replaced onboard Wifi/BT card |
| Power Supply | Corsair HX 1000i | |
| Case | Lian Li PC 011 Dynamic XL | |

### PCIe Slot Layout
| Slot | Speed | Device | Notes |
| ----- | ----- | ---------------------------------------|-------------------|
| 1 | x16 | ASUS ROG Strix RTX 3090 OC | No graphics acceleration, disabled in macOS |
| 2 | x4 | | Slot is disabled due to M.2_2 |
| 3 | x16 | | |
| 4 | x4 | Sapphire RX 580 Pulse 8 GB | Slot running at x4 due to DIMM.2_2 |

### M.2 Layout
| Slot | Device | Notes |
| ----- | ---------------------------------------|-------------------|
| M.2_1 | | Lower slot |
| M.2_2 | Samsung 970 EVO Plus 2 TB | Higher slot |
| DIMM.2_1 | | |
| DIMM.2_2 | Samsung 970 EVO 1 TB | |

## What Works / What Doesn't Work
- [x] Sleep / Wake
- [x] Wifi and Bluetooth
- [x] Handoff, Continuity, AirDrop, Continuity Camera, and Unlock with Apple Watch
- [x] iMessage, FaceTime, App Store, iTunes Store
- [x] 1G Ethernet
- [x] 10G Ethernet
- [x] HEVC, H.264
- [x] Onboard audio
- [x] TRIM
- [x] USB 2.0
- [x] USB 3.2 Gen 1
- [x] USB 3.2 Gen 2
- [x] USB 3.2 Gen 2x2
- [x] DRM
- [x] Native NVRAM
- [x] CPU Power Management
- [x] USB Power
- [ ] SideCar
    * Due to some T2 chip dependancies on MacPro7,1 and iMacPro1,1 SMBIOS

## Screenshots
### USB Mapping
`USBMap-ROG Rampage VI Extreme Encore.zip` contains a full USB Mapping kext for the XHCI controller, both ASMedia USB 3.2 Gen 2 Controllers, and the ASMedia USB 3.2 Gen 2x2 Controller.  The Rampage VI Extreme Encore has a total of 18 ports on the XHCI controller so it's best to remove at least 3 ports to be at the 15 port limit.
![](/Archived/ROG%20Rampage%20VI%20Extreme%20Encore/Images/usbmapping.png)

## Changelog
### Update to OpenCore 0.6.7 (2021.03.01)
### Initial EFI Build (2021.02.22)
