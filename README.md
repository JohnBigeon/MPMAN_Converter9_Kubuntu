# MPMAN_Converter9_Kubuntu

## Concept

## Hardware
MPMAN Converter9 32GB

## Requirements
USB hub
USB key
keyboard

## Install Kubuntu 20.04 lts

### BOOT
Create a boot with Rufus (Windows) of your favorite flavour of Ubuntu, here KUbuntu 20.04 LTS (kubuntu-20.04.4-desktop-amd64.iso).


### BIOS & EFI
In **Security**/**Secure Boot** menu, disable *Secure Boot* and *Secure Boot Mode*.
In **Boot**, disable *Quiet Boot* and *Fast Boot*.
In **Advanced**/**OS/BOM Configuration**, Fix *ISP PCI Device Selection* to *ISP PCI Device as B0D3F0*.

Finally, in **Save & Exit**, select *UEFI: USB DISK* to boot with your USB.

### Fix touchscreen
