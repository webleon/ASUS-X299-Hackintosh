# Personal Build
ASUS Pro WS X299 Sage II

![](/Personal%20Build/Images/ProWSX299SageII.png)

## Specifications
### Components

| Component        | Model                                | Notes |
| ---------------- | ---------------------------------------|-------------------|
| Motherboard | ASUS Pro WS X299 Sage II | BIOS 0901 - Thunderbolt USB 3.0 ports do not work on 1005 with flashed card |
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
| 1 | x16 | | |
| 2 | x8 | | |
| 3 | x8 | | |
| 4 | x8 | | |
| 5 | x8 | AMD Radeon Pro W5500 | |
| 6 | x8 | | |
| 7 | x8 | Intel X550-T2 10G Ethernet Card | |

### M.2/U.2 Layout
| Slot | Device | Notes |
| ----- | ---------------------------------------|-------------------|
| U.2_1 | Gigabyte GC-Titan Ridge V2.0 | Using U.2 to M.2 Adapter + M.2 Riser |
| U.2_2 | | |
| U.2_3 | | |
| M.2_1 | Broadcom BCM943602CDP | Using [M.2 NGFF Adapter](https://www.amazon.com/gp/product/B07R3XVD54/ref=ppx_yo_dt_b_asin_title_o01_s00?ie=UTF8&psc=1) |
| M.2_2 | Sabrent Rocket 1 TB | |

## What Works / What Doesn't Work
- [x] Sleep / Wake
    * Wake via Bluetooth does not work since using M.2 NGFF adapter for Bluetooth.  Using SSDT-GPRW patch to fix instant wake.
- [x] Wifi and Bluetooth
- [x] Handoff, Continuity, AirDrop, Continuity Camera, and Unlock with Apple Watch
- [x] iMessage, FaceTime, App Store, iTunes Store
- [x] 2.5G Ethernet
- [x] 10G Ethernet
    * Modified EEPROM to work with SmallTree drivers. Refer to section [Intel 10 Gigabit NICs with Small Tree macOS Drivers](https://github.com/shinoki7/ASUS-X299-Hackintosh/tree/main/Intel%2010G%20SmallTree) for more info.
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
- [x] Thunderbolt 3/4 hot-plug
- [ ] SideCar
    * Due to some T2 chip dependancies on MacPro7,1 and iMacPro1,1 SMBIOS

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
