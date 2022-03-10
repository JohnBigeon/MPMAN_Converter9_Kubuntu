# MPMAN_Converter9_Kubuntu

## Concept

## Hardware
MPMAN Converter9 32GB
This tablet PC runs an Intel Core Atom Z3735F and has 2 GB of RAM.

## Requirements
USB hub
USB key
keyboard

## Install Kubuntu 20.04 lts

### BOOT
Create a boot with Rufus (Windows) of your favorite flavour of Ubuntu, here KUbuntu 20.04 LTS (kubuntu-20.04.4-desktop-amd64.iso).

#### 32-bit UEFI system
We need to modify the USB so that it will boot on a 32-bit UEFI only system.
The directory **/EFI/BOOT/** needs to contain a *bootia32.efi*, here: [https://github.com/hirotakaster/baytail-bootia32.efi/blob/master/bootia32.efi].
While Windows is running or on another machine, insert the USB and find a directory called /EFI/BOOT. This needs to contain a bootia32.efi file (even if there is one there already it might not be one that works - last time I found one but had to replace it)
Drop this file into the right place (remember all the file and directory names are case sensitive) and your USB should boot, given appropriate UEFI settings (secure boot off, USB first or selected in boot menu)


### BIOS & EFI
In **Security**/**Secure Boot** menu, disable *Secure Boot* and *Secure Boot Mode*.
In **Boot**, disable *Quiet Boot* and *Fast Boot*.
In **Advanced**/**OS/BOM Configuration**, Fix *ISP PCI Device Selection* to *ISP PCI Device as B0D3F0*.

Finally, in **Save & Exit**, select *UEFI: USB DISK* to boot with your USB.

### Fix touchscreen
