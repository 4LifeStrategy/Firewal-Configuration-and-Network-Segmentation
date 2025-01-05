<div align="center" style="white-space: nowrap;">
  <img src="https://github.com/4LifeStrategy/4LifeStrategy/blob/88ffe3009f1399de4502d4d5641c8f7a0fd56852/4LifeStrategy%20Logo%20Center.png" alt="4LifeStrategy Logo" width="100" style="display:inline-block; vertical-align:middle; margin-right:10px;">
  <h1 style="margin:0; vertical-align:middle;">pfSense Segmentation</h1>
</div>

## Description

**pfSense Segmentation** is how I configured 3 separate lans networks.This would segment the network by trusted devices, service devices like server, and iot devices.

## Project Scope

**Objective**: To have iot devices, trusted devices, and server separated from each other on their own lan network and firewall rules to control which devices are able to communicate with each other on the network.

## Tools

**Hardware**:  
- [FW4C – 4 Port Intel® J3710](https://protectli.com/product/fw4c/)  
  - Memory: 8GB  
  - Storage: 120GB SSD  
  - BIOS: [coreboot](https://github.com/coreboot/coreboot)
- USB Flash Drive
  - Capacity: 8GB

**Software**:  
- Firewall/Gateway: [pfSense](https://www.pfsense.org/)
- OS Image Flasher: [balenaEtcher](https://etcher.balena.io/) or [Rufus](https://rufus.ie/en/)

## Instructions

To configure a bootable pfSense usb flash drive you will need to have either balenaEtcher or Rufus to flash a copy of phSense to the flash drive. Linked is the instruction to download pfSense's installation image [AMD64 Memstick USB](https://docs.netgate.com/pfsense/en/latest/install/download-installer-image.html). Next use a OS Image Flasher like blenaEtcher to flash the file to the flash drive. This demonstration is using balenaEtcher.

1. Insert a USB flash drive into the client computer
2. Start blenaEtcher <br /><img src="https://github.com/4LifeStrategy/pfSense-Segmentation/blob/9190a58af2b224ab1b8225581017e6f04b259aa6/balenaEtcher_start.png" width="500">
3. Click **Flash from file**
4. Click the flash drive to which blenaEtcher should write the image
5. Click **Select(1)** to continue
6. Click **Flash!** to write the image to the target USB flash drive
7. Wait for the flash process to complete
8. Close Etcher when complete
9. Remove the USB flash drive from the client system

Then take the flash drive with pfSense and plug it into the [FW4C – 4 Port Intel® J3710](https://protectli.com/product/fw4c/).
