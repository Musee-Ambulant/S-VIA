# Create custom interface for Raspberry pi 4 with qt5 creator

Based on the comprehensive youtube video titled : "Qt for Raspberry Pi - Qt 5.14.2 cross compilation for Raspberry Pi 4 model B - Run Qt on Raspberry" by Ulas Dikme : https://youtu.be/TmtN3Rmx9Rk


## Prerequisites

- [x] Raspberry pi 4 model B (Rpi 4B)
- [x] Minimum 16 Gb micro SD card
- [x] Screen connected to Raspberry pi
- [x] Recommended Raspberry pi 4 model B power adapter
- [x] Raspberry pi IP number reserved via your router dashboard
- [x] Laptop connected the same network as your rpi 4B (PC, MAC or Linux)

## Installation overview

1. Prepare Raspberry Pi for compilation
2. Setup Ubuntu 20.04 virtual machine and prepare for compilation
3. Install QtCreator and deploy compiled example

### Write Raspberry Pi OS to SD card
- Download Raspberry Pi Imager : https://www.raspberrypi.org/downloads/, select imager based on your operating system
- Install software
- Run Raspberry Pi Imager, choose "Raspberry Pi OS" with Raspberry pi Desktop and write to SD card
- Insert SD card into the rpi and boot Rpi

### Basic Rpi setup
Once your Rpi has booted, you will be greated with the installation welcome screen.  Go through the following steps :
- Click on next
- Set Country, language, timezone.  Click Next.
- Set your password
- Connect to your wifi network.  **HOWEVER IT'S RECOMMENDED TO CONNECT WITH A WIRED CONNECTION.**
- Skip the following steps without updating, we will do this in the following steps.


### Open a terminal window
Insert the following commands:
```
sudo apt update
sudo apt-get upgrade
```










# Other Rpi configurations to make your GUI look more professional

## Make screen black on boot
```
sudo nano /boot/cmdline.txt
```
**Add this to the end of the line.**  Make sure it's all on one line or else you will break your Rpi boot sequence.
```
consoleblank=1 logo.nologo quiet loglevel=0 plymouth.enable=0 vt.global_cursor_default=0 plymouth.ignore-serial-consoles splash fastboot noatime nodiratime noram
```
