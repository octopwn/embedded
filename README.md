# embedded
OctoPwn Embedded device information

# Important info
Version 0.1 of the embedded octopwn hardware is preconfigured to use the USB port on the Raspberry PI as USB-OTG client by default. This means that you can't connect any devices (eg. mouse/keyboard) to the microUSB slot.  
General rule of thumb: Do NOT connect anything to either microUSB ports on the device.

# Hardware Descritpion
The OctoPwn Embedded device is a stock [Raspberry Pi Zero 2W](https://www.raspberrypi.com/products/raspberry-pi-zero-2-w/) with an [USB Stem](https://www.sparkfun.com/products/14526) soldered to its USB pads placed in a custom 3D printed plastic case.  

# Operating System
The OS is a stock [Ubuntu Server 23.04 32bit](https://ubuntu.com/download/raspberry-pi)

# Pripherals
After booting the OS, the following peripherals will be active:
 - HDMI (by default, you can connect the microHDMI to a monitor so see what's happening on the device)
 - USB-OTG (virtual keyboard and USB-Ethernet device will appear on the Windows machine if you plugged it into one)

# Network layout
IP address of the device: 10.0.0.1
The networkin is set up so you'll have easy access to the target device -via the USB-Ethernet interface- from the WLAN.  
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

# Network services
The following network services will be available after boot

### DHCP
Operating on br0 the DHCP server is configured to serve 10.0.0.2-254

### SSH
SSH service can be reached from all interfaces, standard port 22/tcp.  
Should anything go wrong, you can connect to WLAN and log in to this service with:  
- user: pi
- pass: octopwn

### SMB
Reachable from all interfaces. No authentication is required on any of the shares.
#### SMB Share - RO
This is a read-only share, you can only upload files to it via SSH. The main purpose of this share is to store the reverse proxy client so in th event the AV might try to remove it the delete operation will fail.
#### SMB Share - Public
This is a read and writeable share backed by the SD card. Use this to transfer files in and out of the target.
#### SMB Share - Update
This is a read and writeable share backed by the SD card. Conents of this share will be checked after each reboot of the device and will update certain parts of the system. More on this later.

### OctoPwn
#### Web service
http://10.0.0.1:80  
To connect to the server, you need to open the url, then select "REMOTE". By default thee is no authentication, so select "NONE" for authentication type.
#### Server
The octopwn server will be listening on ws://10.0.0.1:8181 without authentication.
#### Reverse client handler
For the reverse connection client a websockets server will be listening on ws://10.0.0.1:8700

### Duckytyper
This service is by default only listens on localhost 1212/tcp. Used by OctoPwn to auto-type commands via the virtual keyboard.
