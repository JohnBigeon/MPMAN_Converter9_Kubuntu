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
wget 'https://github.com/onitake/gsl-firmware/raw/master/firmware/linux/silead/gsl1680-mpman-converter9.fw'
````
After a reboot, the touchscreen should be enable.

#### Calibration
For calibration of your touchscreen, use the command:
````
xinput_calibrator
````
#### Right click emulation
Use the great package here [https://github.com/PeterCxy/evdev-right-click-emulation]:
````
sudo apt install git
sudo apt-get install build-essential
sudo apt-get install libevdev-dev
````

git clone 'https://github.com/PeterCxy/evdev-right-click-emulation.git'
````
cd evdev-right-click-emulation
make all
````
In my case, this script returns an error about libraries:
````
uinput.c:(.text+0x1c): undefined reference to `libevdev_new'
````

As mentioned here [https://github.com/PeterCxy/evdev-right-click-emulation/issues/15], update the Makefile [https://github.com/PeterCxy/evdev-right-click-emulation/commit/06c9506ce8cbb4d741f852359d7b77e300b12e49]:
````
nano Makefile
````
````
CFLAGS := $(XFLAGS) $(LIBRARIES) $(INCLUDES) ==> CFLAGS := $(XFLAGS) $(INCLUDES)
$(CC) $(CFLAGS) $^ -o $@ ==> $(CC) $(CFLAGS) $^ $(LIBRARIES) -o $@
````
Now, you should be able to use the right click emulation via the command:
````
out/evdev-rce
````
##### Autostart
To launch the script automatically at the startup, follow the procedure described here [https://fmirkes.github.io/articles/20190827.html]:
Copy in local and 
````
sudo cp 'out/evdev-rce' '/usr/local/bin/'
sudo chmod +x '/usr/local/bin/evdev-rce'
````
Evdev-rce as normal user (update the username !):
````
sudo usermod -G 'input' -a username
````

After a reboot:
````
echo 'uinput' | sudo tee -a /etc/modules
````
````
sudo nano /etc/udev/rules.d/99-uinput.rules
````
````
KERNEL=="uinput", MODE="0660", GROUP="input"
````
````
sudo udevadm control --reload-rules
sudo udevadm trigger
````
````
mkdir -p "$HOME/.config/autostart"
nano "$HOME/.config/autostart/evdev-rce.desktop"
````
Add:
````
[Desktop Entry]
Version=1.0
Type=Application
Name=evdev-rce
GenericName=Enable long-press-to-right-click gesture
Exec=env LONG_CLICK_INTERVAL=500 LONG_CLICK_FUZZ=50 /usr/local/bin/evdev-rce
Terminal=false
StartupNotify=false
````
##### Autorotation
Not yet.

### Audio
As mentioned here in this excellent post [https://ianrenton.com/guides/install-linux-on-a-linx-1010b-tablet/], to fix the sound, you need to edit the alsa-base.conf file
````
sudo nano /etc/modprobe.d/alsa-base.conf
````
Add the line:
````
snd-intel-dspcfg dsp_driver=2
````
and after a reboot, the sound is now working fine.

### Install directly Kubuntu >20.04
Error after the first reboot:
````
/boot/grub/i386-pc not found
````
