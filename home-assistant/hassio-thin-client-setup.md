# Setup Home Assistant on a thin client internal SSD
This guide illustrates how to use a live session from an Xubuntu USB drive running directly on the thin client to install Home Assistant Operating System on the thin client.  

## Overview
A used thin client PC is a low cost alternative to a Raspberry Pi or a Home Assistant Blue devices for running [Home Assistant](https://www.home-assistant.io/). With a low power x86 64-bit processor and upgradeable RAM and SSD in a small case, it's a good choice option for software that runs 24x7 like Home Assistant. With internal mSATA or M.2 SATA SSD drive, installation requires a few more steps as described below.

**Hardware required**
- Thin client with 32GB or larger internal SSD drive that Home Assistant will be installed on
- 4GB or larger USB stick/flash drive for Xubuntu OS image
- 4GB or larger USB stick/flash drive for Home Assistant OS image

**Software required**
- Xubuntu OS ([free download at xubuntu.org](https://xubuntu.org/download/))
- balenaEtcher ([free download at balena.io](https://www.balena.io/etcher/)) for creating a bootable Xubuntu OS USB drive

### Home Assistant Container vs Home Assistant OS
**Home Assistant Operating System** is the recommended installation method. It includes a mimimal Operating System optimized to power Home Assistant. It comes with Supervisor to manage Home Assistant Core, Add-ons, and backups support.  
**Home Assistant Container** is a standalone container-based (e.g. Docker) installation method of Home Assistant Core. This is a good choice if you're using only Home Assistant Core functionality, but this installation method doesn't support popular Add-Ons like [ESPHome](https://esphome.io).  
You'll need a dedicated PC or device to run Home Assistant OS, unlike Home Assistant Container which could be installed on a PC along side other containerized apps. A thin client is a good choice for a PC to run Home Assistant OS as explained in the next section.

### Thin client explained
Thin clients are low power PCs originally designed for office remote desktop use. Used models from corporate leases can be found in good condition on eBay for $50 to $200. Choose a model 3 to 8 years old with dual or quad core x86 64-bit processor, upgradeable RAM, and upgradeable storage.

Benefits of running [Home Assistant](https://www.home-assistant.io/) on a used thin client are:
- Low cost ($50 or less for older models)
- Faster x86 64-bit processor 
- Upgradeable RAM and SSD storage
- Low power consumption (10W) vs full desktop PC (200W)
- Silent (fanless)
- Smaller size than full desktop PC (typically the size of a large WiFi router)
- Has internal Mini PCIe or M.2 expansion slots for adding [Coral AI](https://coral.ai/) accelerator cards useful for running [Frigate NVR](https://frigate.video/)

As of 2022, a good option is a used HP t620 or Dell Wyse 5060 thin client with an upgraded SATA SSD.

|                   | Dell Wyse 5060 thin client                    | HP t620 thin client                         | Home Assistant Blue                     |
| ----------------- | --------------------------------------------- | ------------------------------------------- | --------------------------------------- |
| Processor         | AMD G-Series GX-424CC (2.40 GHz, Quad core)   | AMD GX-415GA (1.5 GHz, Dual core)           | 6-Core Amlogic S922X Processor (ARM v8) |
| RAM               | 4 GB / 8 GB DDR3 (upgradeable)                | 2GB / 4 GB DDR3(upgradeable)                | 4 GB DDR4                               |
| Storage           | 16 GB / 32 GB / 64 GB SATA SSD  (upgradeable) | 16 GB / 32 GB / 64 GB mSATA or M.2 SATA SSD | 128 GB eMMC                             |
| Network           | 1x  Gigabit Ethernet                          | 1x  Gigabit Ethernet                        | 1x Gigabit Ethernet                     |
| Expansion         | 2x USB 3.1 + 4 USB 2.0 + M.2 2230 E key       | 2x USB 3.0 + 6 USB 2.0 + Mini PCIe slot     | 4 USB 3.0                               |
| Power consumption | 13W idle, 17W running                         | 8W idle, 11W running                        | 2W idle, 6W running                     |
| Cost              | $50-$100 USD                                  | $30-$50                                     | $146 USD                                |

### SSD types explained: SATA vs mSATA vs M.2 (NGFF) vs NMVe 
As SSD technologies improved, SSD drives used different connectors. Depending on the model, thin client may use an internal SSD drive with SATA, mSATA or M.2 (NGFF) SATA interface. If upgrading the internal SSD drive in a thin client, double check you're buying a compatible new SSD. **A new M.2 NMVe 2280 SSD will not work in most older thin client devices.**

SSD drives come in the following types, oldest to newest type
1. SATA
2. mSATA (mini-SATA)
3. M.2 (NGFF) SATA
4. M.2 NVMe

**SATA** = same connector as laptop hard disk drives, limited to 6 Gbps peak transfer rate.  
**mSATA (mini-SATA)** = laptop internal add-in card size, uses the same SATA interface with the same peak transfer rate.  
**M.2 (NGFF) SATA** = newer standard for internal add-in cards with different module lengths (ex: 2242 or 2280), but uses the same SATA interface with the same peak transfer rate.  
**M.2 NVMe** = fastest transfer rates (up to 32 Gbps peak), same size internal add-in cards with different module lengths (ex: 2242 or 2280)  

As of 2022, most older thin clients only support M.2 SATA SSD and not M.2 NVMe SSD. M.2 (NGFF) SATA and M.2 NVMe SSD have similar size and shape, but have different electrical interfaces. A motherboard may only support one but not the other, so double check the thin client specifications.   

For more information, see [What is M.2? Keys and Sockets Explained](https://www.atpinc.com/blog/what-is-m.2-M-B-BM-key-socket-3)  

## Instructions
The official Home Assistant installation instructions are written for flashing the Home Assistant OS image directly to the internal SSD drive, which requires specialized USB adapters if the internal drive uses mSATA or M.2 SATA interface. This guide illustrates how to use a live session from an Xubuntu USB drive running directly on the thin client to install Home Assistant Operating System on the thin client.

Installation is separated into a few phases
1. (Optional) Create backup of existing Home Assistant installation if migrating to a different device
2. Create bootable Xubuntu USB drive and copy Home Assistant OS image to USB drive
3. Boot Xubuntu live session on thin client from Xubuntu USB drive
4. Copy Home Assistant OS image onto the thin client's internal SSD drive
5. Start up Home Assistant OS on the thin client 

### (Optional) Create backup of existing Home Assistant installation if migrating to a different device
If you are already running Home Assistant OS or Supervised on a device, then you can create a backup first and restore it on your new device and skip having to configure everyting a second time.

1. Create a backup from Home Assistant UI. See [Home Assistant Backups documentation](https://www.home-assistant.io/common-tasks/supervised/#backups)
2. Download `.tar` backup file to your PC

**Note:** Home Assistant Core and Home Assistant Container installation methods do not include built-in backups feature. You will need to go through setup & configuration again on your new device, but you can manually copy/paste .yaml configuration files. 

### Create bootable Xubuntu USB drive and copy Home Assistant OS image to USB drive
1. Download Xubuntu OS image ([free download at xubuntu.org](https://xubuntu.org/download/))
2. Download balenaEtcher ([free download at balena.io](https://www.balena.io/etcher/))
3. Create a bootable Xubuntu USB drive by following [Ubuntu tutorial](https://ubuntu.com/tutorials/install-ubuntu-desktop#3-create-a-bootable-usb-stick)
4. Download [Home Assistant Operating System image for Generic x86-64 (e.g. Intel NUC)](https://www.home-assistant.io/installation/generic-x86-64) and copy the `haos_generic-x86-64.img.xz` file directly to a separate USB drive. DO NOT directly follow the instructions and use balenaEtcher to flash Home Assistant OS image to your USB drive. (Those instructions are written for flashing the Home Assistant OS image directly to the internal SSD drive.)  

*Note:* Home Assistant Operating System is the recommended installation method, but these instructions will also work for other installation methods like Home Assistant Supervised.  

### Boot Xubuntu live session on thin client from Xubuntu USB drive
1. Connect thin client to a monitor, keyboard, mouse, and wired ethernet cable.
2. Plug in the Xubuntu OS USB drive. 
3. Power on the thin client and press DEL key to enter BIOS settings screen.
4. If prompted, enter BIOS password (Default password for Dell thin client is `Fireport`)
5. Set the boot mode to `UEFI`.
6. Disable `Secure Boot`
7. Change boot order to boot from the USB drive before the internal SSD drive
8. Save BIOS changes and restart
9. Thin client now boots from Xubuntu USB drive and you should see Xubuntu logo on the screen.
10. In the Xubuntu welcome screen, select the "Try Xubuntu" option to start a live session. Complete the setup wizard and you should see Xubuntu desktop.

### Copy Home Assistant OS image onto the thin client's internal SSD drive
1. Plug in the 2nd USB drive with Home Assistant OS `haos_generic-x86-64.img.xz` file
2. Launch a Notes window
3. Launch the Terminal app to open a command line window
4. Enter `df -h` command to list storage device paths. 
5. Locate the path to the 2nd USB drive with Home Assistant OS *.xz file
6. Locate the path to the internal SSD drive in your thin client
7. In Terminal, enter `sudo xz -dc /path/to/haos_generic-x86-64.img.xz | sudo dd of=/path/to/internal/ssd bs=4M conv=fsync` command to copy the Home Assistant OS image from your USB drive to the internal SSD in your thin client.
8. After `xz` copy is completed, shut down Xubuntu and power off the thin client. Unplug both USB drives. You won't need them anymore.

### Start up Home Assistant OS on the thin client 
1. Make sure the wired ethernet cable is plugged in, and power on the thin client. 
2. Thin client should boot from internal SSD drive and start Home Assistant OS. If not, check boot order in the BIOS settings.
3. Home Assistant Operating System will install, and after a few moments, you'll be able to reach Home Assistant on http://homeassistant.local:8123. 

## Conclusion
Congratulations on successfully installing Home Assistant OS on a thin client!  
You can follow [Onboarding Home Assistant guide](https://www.home-assistant.io/getting-started/onboarding/) to continue setup. If you are migrating from another device, you can restore from a `.tar` backup file and have all your configuration and settings restored in seconds.