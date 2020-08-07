# openwrt-pi4

* OpenWRT Raspberry Pi4 4/8GB
* BT Whole Home for wifi mesh
* USB 3.0 SSD in place of microSD


## prepare pi4 for usb boot

1. Download and image to microSD current Raspbian OS

```Shell
wget https://downloads.raspberrypi.org/raspios_lite_armhf_latest
```
Copy the file to the microSD, note check that you have the correct device, should be something like mmcblk0 for the SD card
```Shell
dd bs=4M if=raspios_lite_armhf_latest of=/dev/mmcblkX conv=fsync
```

2. Insert the micoSD and boot the system and login, default user name is pi, with password raspberry
3. Perform the update to raspbian

``` Shell
sudo apt update && sudo apt upgradesudo 
sudo rpi-update 
sudo reboot
```

How to compile OpenWRT for Raspberry Pi4 (64bit) with USB NIC support:
 - No wireless using BT mesh 
 - Support for USB3 Ethernet Adapter


 Steps:

 Setup the Build Environment

 1. Login into OS after installation.  Open Terminal Window and type the following commands:
 2. sudo apt-get update
 3. sudo apt-get install git-core subversion mercurial build-essential libssl-dev libncurses5-dev unzip gawk zlib1g-dev
 4. unset SED
 5. unset GREP_OPTIONS
 6. export GREP_OPTIONS=
 7. export PATH=$PATH:~/openwrt/openwrt/staging_dir/host/bin 
 8. export PATH=$PATH:~/openwrt/openwrt/staging_dir/toolchain-mips_34kc_gcc-5.3.0_musl-1.1.16
 9. Install sublime text 3 | www.sublimetext.com
 10. Install Balena Etcher | https://www.balena.io/etcher

Install OpenWRT Source from GIT:

1. git clone https://github.com/openwrt/openwrt.git

From OpenWRT Source Tree

1. cd openwrt
2. ./scripts/feeds update -a
3. ./scripts/feeds install -a

To create a custom banner:

1. Generate ACSII text banner.
2. Edit openwrt/package/base-files/files/etc" and paste your ACSII text

Run OpenWRT Configuration:

1. make menuconfig

Items we need in OpenWRT Configuration:

1. Target System = Broadcom BCM27xx
2. Subtarget = BCM2711 boards 64bit
3. Target Profile = Raspberry Pi 4b
4. Target Images = squashfs
5. Kernel Modules -
   -- USB Support = kmod-usb-hid
         = kmod-usb-net
         = kmod-usb-net-asix
         = kmod-usb-net-asix-ax88179
         = kmod-usb2
         = kmod-usb3
6. Libraries
   -- libssh
   -- libssh2

7. LUCI -
   -- Collections -
        = luci
        = luci-ssl-openssl
8. Save
9. Exit
10. make (or to use multiple cores) make -jx (where x = cpu cores+1)


How to burn the image to your card and boot
1. Download and install Etcher: https://www.balena.io/etcher
2. Burn the image file from /openwrt/bin/targets/bcm27xx/


How to Overclock
1. Downlaod and install Sublime 3: www.sublimetext.com
2. Edit config.txt
3. Add:

[pi4]
over_voltage=6
arm_freq=2147



How to create a new SSH key pair (private/public)
1. ssh-keygen
2. pick location to save
3. enter passphrase (optional)
4. Using Web Interface, go to the Administration tab
5. Under Services sub-tab, Enable SSHd in the Secure Shell Section
6. Paste your public key in the authorized key of the SSHD section that has now expanded.  
7. Save and Apply settings



How to login and setup the WAN interface:

ref: https://medium.com/openwrt-iot/lede-o...

1. Login into the web interface: https://192.168.1.1
2. Network - Interfaces - Add new interface
3. WAN and select eth1 from interface list.
4. Select DHCP Client as protocol
5. Select WAN as FW zone in FW setting tab
