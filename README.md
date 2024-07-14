# embedded
OctoPwn Embedded device information

# SAFETY INFORMATION - READ THIS!
1. Version 0.1 of the embedded octopwn hardware is preconfigured to use the USB port on the Raspberry PI as USB-OTG client by default. This means that you can't connect any devices (eg. mouse/keyboard) to the microUSB slot.  
2. General rule of thumb: Do NOT connect anything to either microUSB ports on the device, especially when the device is powered on via the stem header!  
3. The custom 3D printed case is designed to hold the device in place even when more-than-usual force is applied to it, also it's printed using carbon fiber reinforced plastic (PLA-CF). Do not try to pry the device out of the case as it ***WILL*** result in the USB stem head tearing off the soldering pads damaging the device. 

# Hardware Descritpion
The OctoPwn Embedded device is a stock [Raspberry Pi Zero 2W](https://www.raspberrypi.com/products/raspberry-pi-zero-2-w/) with an [USB Stem](https://www.sparkfun.com/products/14526) soldered to its USB pads placed in a custom 3D printed plastic case.  

# Operating System
The OS is a stock [Ubuntu Server 24.04 64bit](https://ubuntu.com/download/raspberry-pi)

# Pripherals
After booting the OS, the following peripherals will be active:
 - HDMI (by default, you can connect the microHDMI to a monitor so see what's happening on the device)
 - USB-OTG (virtual keyboard and USB-Ethernet device will appear on the Windows machine if you plugged it into one)

# Network layout
IP address of the device: 10.0.0.1
The networking is set up so you'll have easy access to the target device -via the USB-Ethernet interface- from the WLAN.  
There are four network adapters present on the device (if usb-ethernet is active)
### WLAN (wlan0)
- SSID: octopwn
- PSK: octopwn123
- DHCP is active (10.0.0.0/24)

### Dummy
It does nothing, only needed to keep the bridge active during boot until other network interfaces becomre ready

### Bridge (br0)
This bridge will make sure that you'll reach the target device on usb0 via the WLAN. Also the DHCP server is configured on this interface serving 10.0.0.2-254

### USB-Ethernet (usb0)
This virtual ethernet device is what the connected machine will be able to communicate to. It is using the brige (br0) interface's IP address of 10.0.0.1.

# Network services
The following network services will be available after boot

### DHCP
Operating on br0 the DHCP server is configured to serve 10.0.0.2-254

### SSH
SSH service can be reached from all interfaces, standard port 22/tcp.  
Should anything go wrong, you can connect to WLAN and log in to this service with:  
- user: octopwn
- pass: octopwn

### SMB
Reachable from all interfaces. ~~No authentication is required on any of the shares.~~ As more and more Windows systems disallow connecting to anonymous SMB shares we have added mandatory authentication to all shares.  

 - Domain: WORKGROUP  
 - User: octopwn  
 - Pass: octopwn  

#### SMB Share - RO
This is a read-only share, you can only upload files to it via SSH. The main purpose of this share is to store the proxy so in th event the AV might try to remove it the delete operation will fail.  

#### SMB Share - Public
This is a read and writeable share backed by the SD card. Use this to transfer files in and out of the target.  
**Note**: The transfer speed is limited by the processing power of the small Raspberry Pi device so don't expect more than 2MB/s.

### OctoPwn
#### Web service
http://10.0.0.1:80  

#### Updating
To update Octopwn (or to extend the license) you'd need to download the offline bundle zip file `octopwn.zip` from our [website](https://octopwn.com) then place it to the `Public` SMB share on the device. After this restart the device. If the update is successful, you'll find that the uploaded zip file is removed from the share.  

# Updating the device
The updates will be devlievered via pre-build raspberry pi images which you can `dd` ontho the SD card, or use the official [Raspberry Pi Imager](https://www.raspberrypi.com/software/)


