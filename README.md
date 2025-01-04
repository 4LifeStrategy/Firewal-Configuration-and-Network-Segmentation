<div align="center" style="white-space: nowrap;">
  <img src="https://github.com/4LifeStrategy/4LifeStrategy/blob/88ffe3009f1399de4502d4d5641c8f7a0fd56852/4LifeStrategy%20Logo%20Center.png" alt="4LifeStrategy Logo" width="100" style="display:inline-block; vertical-align:middle; margin-right:10px;">
  <h1 style="margin:0; vertical-align:middle;">pfSense Segmentation</h1>
</div>

## Description

**pfSense Segmentation** is how I configured 3 separate lans networks.This would segment the network by trusted devices, service devices like server, and iot devices.

## Project Scope

**Objective**: To have iot devices, trusted devices, and server separated from each other on their own lan network and firewall rules to control which devices are able to communicate with each other on he network.

## Tools

**Hardware**:
<br/>[FW4C – 4 Port Intel® J3710](https://protectli.com/product/fw4c/)
- Memory: 8GB
- Storage: 120GB SSD
- BIOS: [coreboot](https://github.com/coreboot/coreboot)<br/>

**Software**:
<br/>[pfSense](https://www.pfsense.org/)<br/>
