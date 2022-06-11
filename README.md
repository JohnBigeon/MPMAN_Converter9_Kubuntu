# MPMAN_Converter9_Kubuntu

## Concept
Here, we will install a 64 bit distribution of Ubuntu on a MPMAN Converter9 32GB.

## Hardware
This tablet PC runs an Intel Core Atom Z3735F and has 2 GB of RAM.

## Requirements
````
- USB hub
- USB key
- keyboard
- Internet connection
````
## Install Kubuntu 20.04 lts

### BOOT
Create a boot with Rufus (Windows) or Unetbootin (Linux) of your favorite flavour of Ubuntu, here KUbuntu 20.04 LTS (kubuntu-20.04.4-desktop-amd64.iso).

#### 32-bit UEFI system
We need to modify the USB so that it will boot on a 32-bit UEFI only system.

The directory **/EFI/BOOT/** needs to contain a *bootia32.efi*, here: [https://github.com/hirotakaster/baytail-bootia32.efi/blob/master/bootia32.efi].

### BIOS & EFI
In order to access BIOS, you must press the BIOS key (*DEL*) during the boot-up process. Then, we need to change few parameters.
- In **Security**/**Secure Boot** menu, disable *Secure Boot* and *Secure Boot Mode*.
- In **Boot**, disable *Quiet Boot* and *Fast Boot*.
- In **Advanced**/**OS/BOM Configuration**, Fix *ISP PCI Device Selection* to *ISP PCI Device as B0D3F0*. (Optional ?)

Finally, in **Save & Exit**, select *UEFI: USB DISK* to boot with your USB.

### Installation
During the installation, an internet connection is required to update packages. The Wifi card should be well recognized.
After the reboot, update your system:
````
sudo apt update
sudo apt upgrade
````

#### Remote access
For remote access from your computer to the Converter9.
Open a terminal on the tablet, install openssh-server and determine the ip's address.
````
sudo apt install openssh-server
ip addr show | grep "inet "
````
The output returns now the ip's address of the tablet:
````
inet aaa.bbb.ccc.dd/ee brd aaa.bbb.ccc.fff scope global dynamic noprefixroute wlo1
````
Now, you have access from your computer with:
````
ssh remote_username@aaa.bbb.ccc.dd
````

### Touchscreen
To install the driver for the touchscreen, download the firmware file here: [https://github.com/onitake/gsl-firmware/tree/master/firmware/linux] and place it in a folder named *silead*. Then copy/paste as:
````
cp -pr silead /lib/firmware
````
or directly:
````
mkdir -p /lib/firmware/silead
cd /lib/firmware/silead
wget 'https://github.com/onitake/gsl-firmware/raw/master/firmware/linux/silead/gsXXX.fw'
````
After a reboot, the touchscreen should be enable.

#### Calibration
For calibration of your touchscreen, use the command:
````
xinput_calibrator
````

##### Autorotation
Working ?

### Audio
Not yet.

### Install directly Kubuntu >20.04
Error after the first reboot:
````
/boot/grub/i386-pc not found
````
