#rt3573sta
=========

Patched version of this ralink driver to handle sk_buff structures properly on 64-bit systems

*Forked from [ashaffer/rt3573sta](https://github.com/ashaffer/rt3573sta) this version should fix some errors when compiling on recent kernels*


##Installation

Use `git clone` to clone this repository to a directory of your choosing. Example:
```
mkdir drivers/
cd drivers/
git clone https://github.com/RD777/rt3573sta/
```

Once the directory has been cloned, procede into compiling and installing the module with `make`:
```
cd rt3573sta/
sudo make
```

If there were no errors and the compilation went smoothly, install:
```
sudo make install
```

Afterwards, load the module:
``` 
sudo modprobe rt3573sta
```

##Loading the module at boot

To load the module at boot you should read the documentation of your distribution.

For Arch:

```
sudo touch /etc/modules-load.d/rt3573sta
sudo echo "rt3573sta" > /etc/modules-load.d/rt3573sta
```
Ubuntu suggests adding the module to the `/etc/modules` file

### Blacklisting rt2800usb

Since recent kernels include the rt2800usb module, which is now supporting the Ralink RT3573 chipset, you should blacklist the rt2800usb module to avoid any problems. Doing so is quite easy by adding a file at `/etc/modprobe.d/`, like so:
```
sudo nano /etc/modprobe.d/wireless-blacklist.conf
```
```
# /etc/modprobe.d/wireless-blacklist.conf
blacklist rt2800usb
```

## Compiling the module automatically after a kernel update

For this to work, you will need to install dkms. [Ubuntu Wiki - DKMS](https://help.ubuntu.com/community/DKMS) [Arch Wiki - DKMS](https://wiki.archlinux.org/index.php/Dynamic_Kernel_Module_Support)

***NOTE: Before you follow these steps be sure to read those articles***

Once you have installed dkms, you can now follow these instructions:

Create a directory at `/usr/src/`. Example:
```
mkdir /usr/src/rt3573sta-1.0/
```
Copy all the contents of this repository to that directory:
```
cd ~/DIRECTORY/WITH/THIS/REPO
sudo cp -a * /usr/src/rt3573sta-1.0/
ls /usr/src/rt3573sta-1.0/
     chips  common  include  iwpriv_usage.txt  Makefile  os  rate_ctrl  README_STA_usb  RT2870STACard.dat  RT2870STA.dat  sta  sta_ate_iwpriv_usage.txt  tools

```
Create a file names `dkms.conf`, and with your favorite editor configure dkms. Example:
``` 
sudo vim /usr/src/rt3573sta-1.0/dkms.conf
```
```
# /usr/src/rt3573sta-1.0/dkms.conf
MAKE[0]="make"
CLEAN="make clean"
PACKAGE_NAME="rt3573sta"
PACKAGE_VERSION="1.0"
BUILT_MODULE_NAME[0]="rt3573sta"
BUILT_MODULE_LOCATION[0]="os/linux/"
DEST_MODULE_LOCATION[0]="/kernel/drivers/net/wireless/"
AUTOINSTALL="yes"
```
Add the module to the dkms tree
```
sudo dkms add -m rt3573sta -v 1.0
```
Build the module
```
sudo dkms build -m rt3573sta -v 1.0
```
Install the module
```
sudo dkms install -m rt3573sta -v 1.0
```





