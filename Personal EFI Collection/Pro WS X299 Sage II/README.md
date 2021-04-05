# Pro WS X299 Sage II

![](/Personal%20EFI%20Collection/Pro%20WS%20X299%20Sage%20II/Images/ProWSX299SageII.png)

## Specifications
### Components
| Component        | Model                                | Notes |
| ---------------- | ---------------------------------------|-------------------|
| Motherboard | ASUS Pro WS X299 Sage II | BIOS 0901 |
| Processor | Intel i9-10980XE | |
| CPU Cooler | Corsair H150i Pro RGB | |
| RAM | 4x16 Corsair Vengeance LPX 3200 Mhz | |
| Boot Drive | Samsung 970 EVO 1 TB | |
| Graphics Card | Sapphire RX 580 Pulse 8 GB | |
| Wifi/Bluetooth Card | Broadcom BCM943602CDP |  |
| Power Supply | Corsair RM 850x | |
| Case | Lian Li PC 011 Dynamic | |

### PCIe Slot Layout
| Slot | Speed | Device | Notes | 
| ----- | ----- | ---------------------------------------|-------------------|
| 1 | x16 | | |
| 2 | x8 | | |
| 3 | x8 | | |
| 4 | x8 | | |
| 5 | x8 | | |
| 6 | x8 | | |
| 7 | x8 | Sapphire RX 580 Pulse 8 GB | |

### M.2/U.2 Layout
| Slot | Device | Notes | 
| ----- | ---------------------------------------|-------------------|
| U.2_1 | | |
| U.2_2 | | |
| U.2_3 | | |
| M.2_1 | Broadcom BCM943602CDP | Vertical slot connected via [adapter](https://www.amazon.com/gp/product/B079NB8J3B/ref=ppx_yo_dt_b_search_asin_title?ie=UTF8&psc=1) |
| M.2_2 | Samsung 970 EVO 1 TB | Lower slot |

## What Works / What Doesn't Work
- [x] Sleep / Wake
    * `Wake from Bluetooth` does not work with Wifi/BT adapter in M.2 slot and has instant wake issues.  To fix instant wake, using [GPRW to XPRW patch](https://dortania.github.io/OpenCore-Post-Install/usb/misc/instant-wake.html) for proper sleep.  If Wifi/BT Adapter is connected to a PCIe slot, sleep/wake works properly.
- [x] Wifi and Bluetooth
- [x] Handoff, Continuity, AirDrop, Continuity Camera, and Unlock with Apple Watch
- [x] iMessage, FaceTime, App Store, iTunes Store
- [x] 2.5 G Ethernet
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
- [ ] SideCar
    * Due to some T2 chip dependancies on MacPro7,1 and iMacPro1,1 SMBIOS
    
## Screenshots
### System Report
![](/Personal%20EFI%20Collection/Pro%20WS%20X299%20Sage%20II/Images/aboutthismac.png)
![](/Personal%20EFI%20Collection/Pro%20WS%20X299%20Sage%20II/Images/memory1.png)
![](/Personal%20EFI%20Collection/Pro%20WS%20X299%20Sage%20II/Images/memory2.png)
![](/Personal%20EFI%20Collection/Pro%20WS%20X299%20Sage%20II/Images/pci.png)

### USB Mapping
`USBMap-Pro WS X299 Sage II.zip` contains a full USB Mapping kext for the XHCI controller and both ASMedia USB 3.2 Gen 2 Controllers.  The Sage II has a total of 21 ports on the XHCI controller so it's best to remove at least 6 ports to be at the 15 port limit.
![](/Personal%20EFI%20Collection/Pro%20WS%20X299%20Sage%20II/Images/usbmapping.png)

## Changelog
### Added GPRW method (2021.03.08)
### Update to OpenCore 0.6.7 (2021.03.01)
### Initial EFI Build (2021.02.22)
