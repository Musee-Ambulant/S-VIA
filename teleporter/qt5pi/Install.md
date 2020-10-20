# Create custom interface for Raspberry pi 4 model B with Qt5

#### Based on the comprehensive youtube video titled : "Qt for Raspberry Pi - Qt 5.14.2 cross compilation for Raspberry Pi 4 model B - Run Qt on Raspberry" by [ Ulas Dikme ](https://www.youtube.com/channel/UCM93TMYG5-WE7tQ1UT-EJKw): https://youtu.be/TmtN3Rmx9Rk. I recommend watching the video while going through the following installation steps to better understand what is actually going on 😜.


### Prerequisites

- [x] Raspberry pi 4 model B (Rpi 4B)
- [x] Minimum 16 Gb micro SD card
- [x] Screen connected to Raspberry pi
- [x] Recommended Raspberry pi 4 model B power adapter
- [x] Raspberry pi IP number reserved via your router dashboard
- [x] Laptop connected the same network as your rpi 4B (PC, MAC or Linux)

### Installation overview

1. Prepare Raspberry Pi for compilation
2. Setup Ubuntu 20.04 LTS virtual machine and prepare for compilation
3. Install QtCreator and deploy compiled example

## PART 1 - Prepare Raspberry Pi for compilation

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
Open sources.list and uncomment the last line. 

Uncomment the following :
#deb-src http://raspbian.raspberrypi.org/raspbian/ buster main contrib non-free $

to it looks like :
deb-src http://raspbian.raspberrypi.org/raspbian/ buster main contrib non-free $
```
sudo nano /etc/apt/sources.list
```
Exit and save
```
CTRL + x
y
Enter
````

Update tour system.  When prompted hit ```y```
```
sudo apt update
sudo apt full-upgrade
sudo reboot
```
 After reboot, open terminal and run firmware update. 
 ```
 sudo rpi-update
```

Click on the Raspberry Pi start menu > Preferences > Raspberry Pi Configuration

![](https://www.raspberrypi-spy.co.uk/wp-content/uploads/2015/09/raspberry_pi_configuration.png)

Go to the INTERFACES tab and make sure the following options are set to **ENABLED** :
- [x] SSH
- [x] SPI

Reboot pi from terminal
```
sudo reboot
```

Open terminal and install the following packages and dependencies.  Make sure all files download and install properly.  If you get an error message, run the command again.
```
sudo apt-get build-dep qt5-qmake
sudo apt-get build-dep libqt5webengin-data
```
```
sudo apt-get install libboost1.58-all-dev libudev-dev libinput-dev libts-dev libmtdev-dev libjpeg-dev libfontconfig1-dev

sudo apt-get install libssl-dev libdbus-1-dev libglib2.0-dev libxkbcommon-dev libegl1-mesa-dev libgbm-dev libgles2-mesa-dev mesa-common-dev

sudo apt-get install libasound2-dev libpulse-dev gstreamer1.0-omx libgstreamer1.0-dev libstreamer-plugins-base1.0-dev gstreamer1.0-alsa

sudo apt-get install libvpx-dev libsrtp0-dev libsnappy-dev libnss3-dev

sudo apt-get install "^libxcb.*"

sudo apt-get install libfreetype6-dev libicu-dev libsqlite3-dev libxslt1-dev libavcodec-dev libavformat-dev libswscale-dev

sudo apt-get install libgstreamer0.10-dev gstreamer-tools libraspberrypi-dev libx11-dev libglib2.0-dev

sudo apt-get install freetds-dev libsqlite0-dev libpq-dev libiodbc2-dev firebird-dev libjpeg9-dev libgst-dev libext-dev libxcb1 libxcb1-dev libx11-xcb1

sudo apt-get install libxcb-sync1 libxcb-sync-dev libxcb-render-util0 libxcb-render-util0-dev libxcb-xfixes0-dev libxrender-dev libxcb-shape0-dev libxcb-randr0-dev

sudo apt-get install libxcb-glx0-dev libxi-dev libdrm-dev libssl-dev libxcb-xinerama0 libxcb-xinerama0-dev

sudo apt-get install libatspi-dev libssl-dev libxcursor-dev libxcomposite-dev lib-xdamage-dev libfontconfig1-dev

sudo apt-get install libxss-dev libxtst-dev libpci-dev libcap-dev libsrtp0-dev libxrandr-dev libnss3-dev libdirectfb-dev libaudio-dev

sudo apt-get install 
```

Create the following file and change the permissions
```
sudo mkdir /usr/local/qt5pi
sudo chown pi:pi /usr/local/qt5pi/
```

## PART 2 - Setup Ubuntu 20.04 LTS virtual machine and prepare for compilation

### VirtualBox installation

Go to your host computer, download and install VirtualBox from the following link:
https://www.virtualbox.org/wiki/Downloads 

### Setup Ubuntu 20.04 LTS
Download ths Ubuntu 20.04 image file here: https://ubuntu.com/download/desktop/thank-you?version=20.04.1&architecture=amd64

I recommend watching **Ulas Dikme**'s video (https://youtu.be/TmtN3Rmx9Rk?t=873) **from minute 14:30 to minute 17:53** for complete instructions on setting up a Ubuntu 20.04 LTS VM on your computer.

Once your Ubuntu 20.04 LTS virtual machine is up and runing open a terminal window and continue with the following instructions.

Update your system
```
sudo apt-get update
sudo apt-get upgrade
sudo reboot
```

Once Ubuntu has rebooted, open a terminal window and ping your Raspberry pi.  You will need to change the following ip address for your ip address.
```
ping 10.0.4.83 -c 5
```
If you can successfully ping you Rpi, connect as root user in ubuntu terminal. 
```
sudo bash
apt-get install build-essential
apt-get install gcc git bison python gperf pkg-config
apt install libclang-dev
```
Create the following directory and give your ubuntu user permission.  In my case my user is *aaron*.  Change *aaron* to your ubuntu user.  Then go to the created directory.
```
mkdir /opt/qt5pi/
chown aaron:aaron /opt/qt5pi
cd /opt/qt5pi/
```

Download, open and install toolchain from linaro
```
wget https://releases.linaro.org/components/toolchain/binaries/latest-7/arm-linux-gnueabihf/gcc-linaro-7.5.0-2019.12-x86_64_arm-linux-gnueabihf.tar.xz

tar xf gcc-linaro-7.5.0-2019.12-x86_64_arm-linux-gnueabihf.tar.xz
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
