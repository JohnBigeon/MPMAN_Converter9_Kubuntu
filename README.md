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
Create a boot with Rufus (Windows) of your favorite flavour of Ubuntu, here KUbuntu 20.04 LTS (kubuntu-20.04.4-desktop-amd64.iso).

#### 32-bit UEFI system
We need to modify the USB so that it will boot on a 32-bit UEFI only system.

The directory **/EFI/BOOT/** needs to contain a *bootia32.efi*, here: [https://github.com/hirotakaster/baytail-bootia32.efi/blob/master/bootia32.efi].

### BIOS & EFI
- In **Security**/**Secure Boot** menu, disable *Secure Boot* and *Secure Boot Mode*.
- In **Boot**, disable *Quiet Boot* and *Fast Boot*.
- In **Advanced**/**OS/BOM Configuration**, Fix *ISP PCI Device Selection* to *ISP PCI Device as B0D3F0*.

Finally, in **Save & Exit**, select *UEFI: USB DISK* to boot with your USB.

### Installation
During the installation, an internet connection is required to update packages.
The Wifi card should be well recognized.

### Fix touchscreen
