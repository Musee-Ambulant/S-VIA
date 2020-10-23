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
Once your Rpi has booted, you will be greated with the installation welcome screen.  Go through the following steps:
- Click on next
- Set Country, language, timezone.  Click Next.
- Set your password
- Connect to your wifi network.  **HOWEVER IT'S RECOMMENDED TO CONNECT WITH A WIRED CONNECTION.**
- Skip the following steps without updating, we will do this in the following steps


### Open a terminal window
Open sources.list and uncomment the last line

Uncomment the following:
#deb-src http://raspbian.raspberrypi.org/raspbian/ buster main contrib non-free $

to it looks like:
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
 After reboot, open terminal and run firmware update 
 ```
 sudo rpi-update
```

Click on the Raspberry Pi start menu > Preferences > Raspberry Pi Configuration

![](https://www.raspberrypi-spy.co.uk/wp-content/uploads/2015/09/raspberry_pi_configuration.png)

Go to the INTERFACES tab and make sure the following options are set to **ENABLED**:
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

Once Ubuntu has rebooted, open a terminal window and ping your Raspberry pi.  You will need to change the following ip address ( *10.0.4.83* ) for your ip address.
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
Create the following directory and give your ubuntu user permission.  In my case my user is ( *aaron* ).  Change ( *aaron* ) to your ubuntu user.  Then go to the created directory.
```
mkdir /opt/qt5pi/
chown aaron:aaron /opt/qt5pi
cd /opt/qt5pi/
```
Download and open toolchain from linaro
```
wget https://releases.linaro.org/components/toolchain/binaries/latest-7/arm-linux-gnueabihf/gcc-linaro-7.5.0-2019.12-x86_64_arm-linux-gnueabihf.tar.xz

tar xf gcc-linaro-7.5.0-2019.12-x86_64_arm-linux-gnueabihf.tar.xz
```
Export to PATH
```
export PATH=$PATH:/opt/qt5pi/gcc-linaro-7.5.0-2019.12-x86_64_arm-linux-gnueabihf/bin
```
Open *bashrc*
```
nano ~/.bashrc
``` 
Add the export line to the end of the file. Exit ```crtl + x``` and Save ```y```
```
export PATH=$PATH:/opt/qt5pi/gcc-linaro-7.5.0-2019.12-x86_64_arm-linux-gnueabihf/bin
```
Download and open qt5.14
```
wget https://download.qt.io/official_releases/qt/5.14/5.14.2/single/qt-everywhere-src-5.14.2.tar.xz

tar xf qt-everywhere-src-5.14.2.tar.xz
```
Rsync the files from the Rpi to the host machine for cross compilation.  Change ip ( *10.0.4.83* ) and Rpi user ( *pi* ) accordingly.
```
rsync -avz pi@10.0.4.83:/lib sysroot
rsync -avz pi@10.0.4.83:/usr/include sysroot/usr
rsync -avz pi@10.0.4.83:/usr/lib sysroot/usr
rsync -avz pi@10.0.4.83:/opt/vc sysroot/opt
```
Move the following files to keep as a backups
```
mv sysroot/usr/lib/arm-linux-gnueabihf/libEGL.so.1.1.0 sysroot/usr/lib/arm-linux-gnueabihf/libEGL.so.1.1.0_backup

mv sysroot/usr/lib/arm-linux-gnueabihf/libEGLSv2.so.2.1.0 sysroot/usr/lib/arm-linux-gnueabihf/libEGLSv2.so.2.1.0_backup
```
Create the following softlinks
```
ln -s sysroot/opt/vc/lib/libEGL.so sysroot/usr/lib/arm-linux-gnueabihf/libEGL.so.1.1.0

ln -s sysroot/opt/vc/lib/libEGLSv2.so sysroot/usr/lib/arm-linux-gnueabihf/libEGLSv2.so.2.1.0

ln -s sysroot/opt/vc/lib/libEGL.so sysroot/opt/vc/lib/libEGL.so.1

ln -s sysroot/opt/vc/lib/libEGLSv2.so sysroot/opt/vc/lib/libEGLSv2.so.2
```
Install, change permissions and run the following Python script
```
wget https://raw.githubusercontent.com/riscv/riscv-poky/master/scripts/sysroot-relativelinks.py

chmod +x sysroot-relativelinks.py

./sysroot-relativelinks.py sysroot
```
Run the rsync commands again
```
rsync -avz pi@10.0.4.83:/lib sysroot
rsync -avz pi@10.0.4.83:/usr/include sysroot/usr
rsync -avz pi@10.0.4.83:/usr/lib sysroot/usr
rsync -avz pi@10.0.4.83:/opt/vc sysroot/opt
```
Run the Python script again
```
./sysroot-relativelinks.py sysroot
```
Create the following directory in the ( */opt/qt5pi/* ) directory which you should still be in.  Go to the new directory.
```
mkdir qt5build
cd qt5build/
```
Copy the following directory recursively to the ( *qt5build* ) directory
```
cp -r /opt/qt5pi/qt-everywhere-src-5.14.2/ /opt/qt5pi/qt5build/
```
Run the following configurations and commands.  The ( *make -j8* ) command will take at least an hour to run, all depending on the ressources you allocated to your virtual machine. 
```
../qt-everywhere-src-5.14.2/configure -opengl es2 -device linux-rasp-pi4-v3d-g++ -device-option CROSS_COMPILE=/opt/qt5pi/gcc-linaro-7.5.0-2019.12-x86_64_arm-linux-gnueabihf/bin/arm-linux-gnueabihf- -sysroot /opt/qt5pi/sysroot -prefix /usr/local/qt5pi -opensource -confirm-license -skip qtscript -skip qtwayland -skip qtdatavis3d -nomake examples -make libs -pkg-config -no-use-gold-linker -v

make -j8
make install
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
