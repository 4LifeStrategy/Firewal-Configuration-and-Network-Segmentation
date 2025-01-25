<div align="center" style="white-space: nowrap;">
  <img src="https://github.com/4LifeStrategy/4LifeStrategy/blob/88ffe3009f1399de4502d4d5641c8f7a0fd56852/4LifeStrategy%20Logo%20Center.png" alt="4LifeStrategy Logo" width="100" style="display:inline-block; vertical-align:middle; margin-right:10px;">
  <h1 style="margin:0; vertical-align:middle;">Firewall Configuration & Network Segmentation</h1>
</div>

## Description

**Firewall Configuration & Network Segmentation** is how I configured 3 separate lans networks.This would segment the network by trusted devices, service devices like server, and iot devices.

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
- Your Internet Provider or personal modem.
- Access Point or wifi router that can be set to bridge mode.
- Network Switch

**Software**:  
- Firewall/Gateway: [pfSense](https://www.pfsense.org/)
- OS Image Flasher: [balenaEtcher](https://etcher.balena.io/) or [Rufus](https://rufus.ie/en/)

## Prepare Installation Media

To configure a bootable pfSense usb flash drive you will need to have either balenaEtcher or Rufus to flash a copy of phSense to the flash drive. Linked is the instruction to download pfSense's installation image [AMD64 Memstick USB](https://docs.netgate.com/pfsense/en/latest/install/download-installer-image.html). Next use a OS Image Flasher like blenaEtcher to flash the file to the flash drive. This demonstration is using balenaEtcher.

1. Insert a USB flash drive into the client computer
2. Start blenaEtcher <br /><img src="https://github.com/4LifeStrategy/Firewal-Configuration-and-Network-Segmentation/blob/162f74642fa8494f00ad1ef8d86a4ac7856b5434/balenaEtcher_start.png" width="500">
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

## Installing pfSense

Now that we are booted into the pfSense installer drive. We can start installing pfSense on to the Protectli Vault hardware. When the installer starts the first screen it presents offers license terms for pfSense® software which must accept before installation. Read the terms carefully. Use the Page Down and Page Up keys to display additional license text. Press **Enter** to Accept the terms and proceed.

1. Press **Enter** on **Install pfSense**<br /><img src="https://github.com/4LifeStrategy/Firewal-Configuration-and-Network-Segmentation/blob/162f74642fa8494f00ad1ef8d86a4ac7856b5434/pfsense_installer_start.png" width="500">
2. Press **Enter** on **Continue with default keymap**
3. Press **Enter** on **Guided Root-on-ZFS**
4. Press **Enter** on **Proceed with Installation**
5. Press **Enter** on **Stripe-No Redundancy**
6. Press **Enter** on **Protectli 120GB mSATA**<br />*It will show a different number depending on the your storage option*
7. Press **Y** to confirm your choice one last time
8. Once the installation is complete press **N** to confirm you don't want to make any changes.
9. Finally press **R** to reboot the device.

## Configuring pfSense

Once the device has rebooted, we can start to configure the pfSense settings. Plug an ethernet cable into the Lan Port of your Protectli Vault and plug the other end into your computer. Open a browser on your computer and go to **192.168.1.1**, it will load to the pfSense login screen.

1. Enter the default username **admin** and the default password **pfsense**

We are going to change the default url. This will add some obscurity to the network by not using defaults.

1. Select **Interfaces**
2. Select **LAN**
3. Scroll down to **Static IPv4 Configuration** delete the IPv4 address and change it to some other arbitrary number in the 192.168 space.<br />*For this demonstration we are using 115 for our 3rd octet.<br />Example: **192.168.115.1***
4. Click **Save** but don't click **Apply changes** yet.<br />*We need to configure the dns server or else pfSense will not be able to automatically assign a new ip address to your computer and causing you to loss the ability to access the pfSense settings again.*
5. Select **Services**
6. Select **DHCP Server**
7. Scroll down to **Range** and change From: **192.168.115.100** and To: **192.168.115.254**<br />*This will let the DHCP server to automatically assigned ip address between 100-254. Leaving ip range 2-99 for static devices.*
8. Click **Save**
9. Select **Interfaces**
10. Select **LAN**
11. Click **Apply changes**

The web page may seem not responsive but its because we confirmed the changes and changed the defaults. Unplug your ethernet cord from your device and re-plug it back on. This will cause your device to reach out to the pfSense dhcp server for a new ip address. On the address bar of your browser type the ip address that you configured. For this demonstration we used **http://192.168.115.1**.

## Connecting modem and wifi

1. Connect your **modem** to the Protectli Vault in the **WAN port**. You may have to restart your modem to working with pfSense. 
2. Connect the **network switch** to the **OTP2 port** on your Protectli Vault.
3. Cnnect your wifi **access point** or **wifi router** to **network switch**.
 
**Network Diagram**<br /><img src="https://github.com/4LifeStrategy/Firewal-Configuration-and-Network-Segmentation/blob/23217f939f554bff64b42bbcf6f19dcf37cd2e2b/Network%20Diagram.png" width="500">

Put the router in Bridge mode, some routers call their bridge mode, access point mode. This mode will bypass it's own dhcp server and rely on pfSense's dhcp server. For this demonstration we are using Google Wifi that but in bridge mode performing the following steps.

1. Within the Google Home app tap on **Wifi**
2. Tap on **Network setting**
3. Tap on **Advanced setting**
4. Tap on **Device mode**
5. Tap on the **Wifi Location**
6. Then select **Bridge mode**
7. The router will perform a restart.

## Network Segmentation

By default OPT1 and OPT2 interface are disabled and we need to enable and configure them.

1. Login the pfSense intense by going to **http://192.168.115.1** on your web browser.
2. Click on **Interfaces**
3. Click on **Assignments**
4. Click the **Add** button twice and it will automatically assign OPT1 and OPT2 interfaces.
5. Click **Save**

Since OPT1 and OPT2 are now enabled. Next we are going to assign a static ip address to the network gateway and setting up the DHCP range for that networks. We are basically performing the same steps we performed for the LAN interface but changing the 3rd octet for each network.

1. Click on **Interfaces**
2. Click on **OPT1**
3. Click the box **Enable Interface**
4. Change **IPv4 Configuration Type** to **Static IPv4**
5. Scroll down to **Static IPv4 Configuration** put some other arbitrary number in the 192.168 space.<br />*For this demonstration we are using 115 for our 3rd octet.<br />Example: **192.168.215.1***
6. Next to the ip address field is a **/**, make sure it's set to **24**
7. In **IPv4 Upstream gateway** set to **None**
8. Click **Save**
9. Click **Apply Changes**

Follow the same steps for OPT2 but changing the 3rd octet. This demo we will be using **192.168.315.1** Next we are going to configure DHCP for OPT1 and OPT2.

1. Select **Services**
2. Select **DHCP Server**
3. Click on the **OTP1** tab
4. Check ***Enable DHCP server on OTP1 Interface**
5. Scroll down to **Range** and change From: **192.168.215.100** and To: **192.168.215.254**<br />*This will let the DHCP server to automatically assigned ip address between 100-254. Leaving ip range 2-99 for static devices.*
6. In **DNS server** put **192.168.215.1**
7. Click **Save**
8. Click on the **OTP2** tab
9. Check ***Enable DHCP server on OTP1 Interface**
10. Scroll down to **Range** and change From: **192.168.315.100** and To: **192.168.315.254**<br />*This will let the DHCP server to automatically assigned ip address between 100-254. Leaving ip range 2-99 for static devices.*
11. In **DNS server** put **192.168.315.1**
12. Click **Save**

**Domain Name Server (DNS) Configuration**

We need to configure dns to be able to utilize the internet.

1. Click on **Services**
2. Click on **DNS resolver**
3. Scroll down to **Network Interfaces** and select **All**
4. In **Outgoing Network Interface** select **WAN** if not already selected.
5. Click **Save**
6. Click **Apply Changes**

**Firewall Rules** 

Next we are going to set up basic firewall rules to block OTP2 (which would be considered the less secure network interface due to hosting IOT devices and over devices connected via wifi) from initiating communication to other interfaces but other network are able to initiate communication. First we are going to setup firewall rules to grant access to the internet for OPT1 and OPT2.

1. Go to **Firewall**
2. Select **Rules**
3. Click on **LAN**
4. Click the **COPY** symbol on the firewall rule with Description **Default allow LAN to any rule**
5. In **Interface** change it from LAN to **OPT1**
6. Under **Source** change it from LAN Net to **OPT1** net.
7. Change the description to state **Default allow OPT1 to any rule**
8. Click **Save**
9. Click **Apply Changes**

Do the same for OPT2 and test each port (LAN, OPT1, and OPT2) are able to reach the internet by connecting your computer to each of the ports and utilize the web browser then go to any site. We are going to add more firewall rules. Keep in mind that firewall rules are executed in order starting from the top and makes it way down for each network packet.

Rule to block connection from LAN to OPT1
1. In the LAN tab Click on the **Add (up arrow)**
2. For **Action** set to **Block**
3. **Protocol** set to **Any**
4. **Source** set to **LAN net**
5. **Destination** set to **OPT1 Net**
6. Check on **Log packets that are handled by this rule**
7. Add a description describing the rule action
8. Click **Save**
9. Click **Apply Changes**

Rule to block connection from OPT2 To OPT1
1. In the OPT2 tab Click on the **Add (up arrow)**
2. For **Action** set to **Block**
3. **Protocol** set to **Any**
4. **Source** set to **OTP2 net**
5. **Destination** set to **OPT1 Net**
6. Check on **Log packets that are handled by this rule**

Rule to block connection from OPT2 To LAN
1. In the OPT2 tab Click on the **Add (up arrow)**
2. For **Action** set to **Block**
3. **Protocol** set to **Any**
4. **Source** set to **OTP2 net**
5. **Destination** set to **LAN Net**
6. Check on **Log packets that are handled by this rule**

## Static IP Address & Aliasing

Assigning static ip addresses for devices on the network would allow maintaining firewalls rules that involves specific devices. Allies allows up to create groups of static ip address to be referenced by a desired naming schema. You will need to find the mac address of each device that you want to assign a static ip address. Usually found in the network settings of said device.

1. Click **Status**
2. Click **DHCP Leases**
3. Locate the mac address of the desired device on the list of devices. To the right of it under the **Action** column, click the plus icon that says **Add static mapping** when the mouse is hovered over the button.
4. In **IP Address** delete the 4th octet with any number between 2-99.
5. In **Hostname** add the desired name for the device
6. Click **Save**
7. Click **Apply changes**<br />*You may need to restart or unplug and replug the device so it can call to the DHCP server for its now assigned ip address.*

Repeat the action each device you want to assign an ip address to a device. For this demonstration we have already assigned ip address to some mobile devices, laptop, nas, and tablet. Next we are going to create some aliases to refer to some of devices when creating firewall rules.

Alias for Trusted_Devices
1. Click **Firewall**
2. Click **Aliases**
3. Click **Add**
4. Under **Name** put **Trusted_Devices**
5. Under **Description** add your description
6. Under **Type** select **Host(s)**
7. Under **IP or FQDN** add the static ip of your personal devices like laptop and mobile devices and add a description for each ip address appropriately.<br /><img src="https://github.com/4LifeStrategy/Firewal-Configuration-and-Network-Segmentation/blob/8bba55d16df7edc5ccbc817ab4516e20f86eff22/Trusted%20Devices.png" width="500">
8. Click **Save**
9. Click **Apply changes**

Alias for Plex Service
Based on [Troubleshooting Remote Access](https://support.plex.tv/articles/200931138-troubleshooting-remote-access/) [current working ip address](https://s3-eu-west-1.amazonaws.com/plex-sidekiq-servers-list/sidekiqIPs.txt)
1. Click **Firewall**
2. Click **Aliases**
3. Click **Add**
4. Under **Name** put **Plex**
5. Under **Description** add your description
6. Under **Type** select **Host(s)**
7. Under **IP or FQDN** add the ip address listed in [current working ip address](https://s3-eu-west-1.amazonaws.com/plex-sidekiq-servers-list/sidekiqIPs.txt)
8. Click **Save**
9. Click **Apply changes**

Alias for Cloudflare
1. Click **Firewall**
2. Click **Aliases**
3. Click **Add**
4. Under **Name** put **Cloudflare**
5. Under **Description** add your description
6. Under **Type** select **Network(s)**
7. Under **Network or FQDN** add the IPv4 networks from [Cloudflare IP Range](https://www.cloudflare.com/ips/)
8. Click **Save**
9. Click **Apply changes**

Repeat the steps for family devices and another for a nas. Next are some rules to allow Trusted_Devices and Family_Devices to be able to reach out to NAS_Services.

Rule to allow Trusted_Devices to NAS_Services
1. Click on **Firewall**
2. Click on **Rules**
3. Click **LAN**
4. Click **Added up arrow**
5. For **Action** select **Pass**
6. For **Protocol** select **Any**
7. For **Sources** select **Address or Alias** then next field type **Trusted_Devices**
8. For **Destination** select **Address or Alias** then next field type **NAS_Services**
9. For **Description** add a description of the rule action.
10. Click **Save**
11. Click **Apply changes**
12. Click the **Copy** symbol on the new rule we created.
13. For **Interface** select **OPT2**
14. Click **Save**
15. Click **Apply Changes**

Rule to Allow Family_Devices to NAS_Services
Click on **Firewall**
2. Click on **Rules**
3. Click **OPT2**
4. Click **Added up arrow**
5. For **Action** select **Pass**
6. For **Protocol** select **Any**
7. For **Sources** select **Address or Alias** then next field type **Family_Devices**
8. For **Destination** select **Address or Alias** then next field type **NAS_Services**
9. For **Description** add a description of the rule action.
10. Click **Save**
11. Click **Apply changes**

## Conditional Port Forwarding

we will use Conditional Port Forwarding to allow traffic from specified source that is calling to our public IP address to pass through the firewall on a specified port and forward that traffic to a specified destination. We will setup up 2 rules to allow Plex Media Services and Cloudflare.

Rule to allow Plex to NAS_Services
1. Click **Firewall**
2. Click **NAT**
3. Click **Add**
4. Under **Protocol** select **TCP**
5. Click **Show Advanced**
6. **Source** select **Address or Alias**
7. Type **Plex**
8. **Source** select **Any**
9. **Destination** select **WAN Address**
10. **Destination port range** select **Other**
11. **From port** add the port that you setup for your Plex Server on the NAS
12. **To port** will be the same number
13. **Redirect target IP** select **Address or Alias**
14. for **Type** type **NAS_Services**
15. **Redirect target port** select **Other**
16. **Port** put the port that you setup for your Plex Server on the NAS
17. **Description** add description
18. Click **Save**
19. Click **Apply changes**

Rule to Allow Cloudflare to Nginx Proxy Manager
1. Click **Firewall**
2. Click **NAT**
3. Click **Add**
4. Under **Protocol** select **TCP**
5. Click **Show Advanced**
6. **Source** select **Address or Alias**
7. Type **Cloudflare**
8. **Source** select **Any**
9. **Destination** select **WAN Address**
10. **Destination port range** select **HTTPS**
11. **Custom** select **HTTPS**
**Redirect target IP** select **Address or Alias**
12. for **Type** type **Nginx_Server**
13. **Redirect target port** select **HTTPS**
14. **Port** put the port that you setup for your Plex Server on the NAS
15. **Description** add description
16. Click **Save**
17. Click **Apply changes**
