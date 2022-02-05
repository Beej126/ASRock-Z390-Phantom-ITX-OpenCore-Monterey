# ASRock-Z390-Phantom-ITX-OpenCore-Monterey
Working settings for OpenCore Monterey on ASRock Z390 Phantom Gaming ITX

## OS Version
- Montery 12.2

## OpenCore Version
- 0.7.7

## Hardware
| Item | Model | Notes |
| --- | --- | --- |
| Motherboard | [Asrock Z390 Phantom Gaming ITX/AC](https://www.asrock.com/MB/Intel/Z390%20Phantom%20Gaming-ITXac/index.asp) | |
| BIOS Version | [4.40](https://www.asrock.com/MB/Intel/Z390%20Phantom%20Gaming-ITXac/index.asp#BIOS) | (not attempting 4.40c thunderbolt bios just yet) |
| CPU | [Intel i9-9900K (Coffee Lake)](https://ark.intel.com/content/www/us/en/ark/products/186605/intel-core-i99900k-processor-16m-cache-up-to-5-00-ghz.html) | |
| GPU | iGPU [Intel UHD 630](https://ark.intel.com/content/www/us/en/ark/products/graphics/126790/intel-uhd-graphics-630.html) | |
| Memory | [Corsair Dominator Platinum DDR4 3200 2 x 16GB (CMD32GX4M2C3200C16)](https://www.corsair.com/us/en/Categories/Products/Memory/DOMINATOR®-PLATINUM-32GB-%282-x-16GB%29-DDR4-DRAM-3200MHz-C16-Memory-Kit/p/CMD32GX4M2C3200C16) | |
| SSD | [Samsung 970 Pro NVMe m.2 SSD 512GB](https://www.samsung.com/us/computing/memory-storage/solid-state-drives/ssd-970-pro-nvme-m2-512gb-mz-v7p512bw/) | |
| WiFi/Bluetooth Module | Broadcom [BCM4360](https://www.broadcom.com/products/wireless/wireless-lan-infrastructure/bcm4360) / [BCM2046](https://devicehunt.com/view/type/usb/vendor/0A5C/device/4500) | |
| Case | [Old School Cyang Fun "gBox" SFF with handle 💖](https://beej126.github.io/gBox-resurrected/) | |
| PSU | [Corsair SF600 SFX - 600watt](https://www.corsair.com/us/en/Categories/Products/Power-Supply-Units/Power-Supply-Units-Advanced/SF-Series/p/CP-9020182-NA) | |
| Cooler | [Corsair H80i v2 - 120mm rad x 2 fans](https://www.corsair.com/us/en/Categories/Products/Liquid-Cooling/Single-Radiator-Liquid-Coolers/Hydro-Series%E2%84%A2-H80i-v2-High-Performance-Liquid-CPU-Cooler/p/CW-9060024-WW) | |
| Monitor | [Samsung UN55NU7300 *CURVED* 55" 4K TV ](https://www.samsung.com/ca/support/model/UN55NU7300FXZC/) | motherboard hdmi |

## config.plist highlights
- boot-args - e.g. verbose is enabled vs seeing clean mac logo and progress bar
- main iGPU platform-id
- drivers
- kexts
- boot menu "picker" settings
- boot menu tools - handy to have clear/reset nvram, toggle sip and open efi shell

## OpenCore guide for desktop Coffee Lake
- https://dortania.github.io/OpenCore-Install-Guide/config.plist/coffee-lake.html#starting-point
- make sure you really [enable/disable all the appropriate bios settings!!](https://dortania.github.io/OpenCore-Install-Guide/config.plist/coffee-lake.html#intel-bios-settings)

## Notes / Tips
- This EFI is iGPU only (i.e. no discrete graphics card settings) which was something i couldn't readily find elsewhere but otherwise this is largely copied from these other EFI bundles i found: [SeanZhang98](https://github.com/seanzhang98/ASRock-Z390-Phantom-ITX-OpenCore-Hackintosh-Monterey/blob/main/README_en.md), [FangF2018](https://github.com/fangf2018/ASRock-Z390-Phantom-ITX-OpenCore-Hackintosh/tree/master/EFI/OC), [BoBo88](https://www.tonymacx86.com/threads/asrock-z390-phantom-gaming-itx-i7-8700k-rx-570-pulse-itx-opencore-0-6-8-bigsur-11-3-windows-10.312078/), [AnsonLiao](https://github.com/ansonliao/EFI-ASRock-Z390-Phantom-Gaming-ITX)
- i had working hackmac already (Catalina on Clover), so preferred to download Monterey from Apple Store and install to another drive versus creating usb boot... the initial install is of course nice and cozy this way but the fun is whether the install runs on your EFI upon boot... mine was freezing on apple logo progress bar a lot till i banged around on opencore configs... classic case of not sure which change really made the difference but i think AnsonLiao's EFI bundle was the closest out of the box.
- ### EFI partition is a handy go between, make it big!
  - macOs format tools default EFI partition to 200MB which can fill up fast if you use it to keep things around between multiple OS's and versions... it's hard to find any real concrete recommendations on maximum size... [here's one saying 950MB is ok](https://www.ujmix.com/how-to-increase-efi-partition-mac/)
  - the best time to do this is PRIOR TO INSTALLING macOS!! after the AFPS partition is created it's non trivial (maybe impossible) to move.
  - contemporary versions of macOs Disk Utility hide the EFI from direct manipulation ... one trick is to erase the disk to get the stock GDI layout with standard 210MB EFI and then carve out another little partition for the EFI to expand into later... this way we get a clean EFI that the macOs installer will proceed with and we have room to create our preferred EFI partition after the installer is done.  I named this partition EFI2 with size of 740MB (i.e. 210 default + 740 = 950 total).
   <img src="https://user-images.githubusercontent.com/6301228/152660575-cfb854bd-dfbc-486b-a3f7-5422cb274b1f.png" width="600px"/>
   <img src="https://user-images.githubusercontent.com/6301228/152660998-836bed5c-8f4c-4b4d-8738-2d40567415a3.png" width="600px"/>
   <img src="https://user-images.githubusercontent.com/6301228/152661042-e74e0b68-38d7-4946-b1fe-ef046a51692a.png" width="600px"/>

## Tools
- [OpenCore](https://github.com/acidanthera/OpenCorePkg/releases)
- [ProperTree](https://github.com/corpnewt/ProperTree) - the safest way to edit config.plist for opencore
- [Mackie's OpenCore Configurator v2.56.00](https://mackie100projects.altervista.org/download-opencore-configurator/) - worked fine for me but OpenCore's docs specifically do specifically say to avoid
- [Hackintool](https://github.com/headkaze/Hackintool/releases/) - good for viewing exact deviceId's and tons of other settings
- [XtraFinder](https://www.trankynam.com/xtrafinder/) - F2 to rename and DEL to delete 💖, dual tab finders side by side
