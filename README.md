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

**Prepare Installation Media**  

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

**Booting the Install Media**  

Insert the flash drive and then power on the target system. In this demonstration we are using [FW4C – 4 Port Intel® J3710](https://protectli.com/product/fw4c/). Then turn the Protectli Vault on. It should bootup into pfsense installation but if it does not follow these steps.

1. Make sure that the Protectli Vault is powered off
2. Power back on the device and while it booting up press **F11** on your keyboard.
3. Choose **Boot From USB**

**Installing pfSense**

Now that we are booted into the pfSense installer drive. We can start installing pfSense on to the Protectli Vault hardware.When the installer starts the first screen it presents offers license terms for pfSense® software which must accept before installation. Read the terms carefully. Use the Page Down and Page Up keys to display additional license text. Press **Enter** to Accept the terms and proceed.

1. Press **Enter** on **Install pfSense**<br /><img src="https://github.com/4LifeStrategy/pfSense-Segmentation/blob/9683b8f96804708b9235488601e255a8e72f5142/pfsense_installer_start.png" width="500">
2. Press **Enter** on **Continue with default keymap**
3. Press **ENter** on **Guided Root-on-ZFS**
4. Press **Enter** on **Proceed with Installation**
5. Press **Enter** on **Stripe-No Redundancy**
6. Press **Enter** on **Protectli 120GB mSATA**<br />*It will show a different number depending on the your storage option*
7. Press **Y** to confirm your choice one last time
8. Once the installation is complete press **N** to confirm you don't want to make any changes.
9. Finally press **R** to reboot the device.

**Configuring pfSense**
