# Create Windows 10 USB from Linux

This repository collects the steps necessary to create a Windows 10 installation media without the need of another Windows installation (e.g. using Rufus). 

## Requirements

- Gparted (or something to manage partitions)
- p7zip-full
- At least 8GB thumb drive

## Partitioning

Create a GPT table on the target device with two partitions:

1. greater than 700MB FAT32 partition (I went for 1024MB, labeled as `UEFI_BOOT`)
2. greater than 6GB NTFS partition (I chose to give that all the space remaining and label it with `Win10` name)

## Create two .txt files for p7zip-full

```
echo "sources/" > exclude.txt
echo "sources/boot.wim" > include.txt
```

## Copy the installation files:

Mount your partitions with non-root privileges (`udisks2` or your file manager of choice, since `mount` requires `sudo`)

```
7z x -x@exclude.txt Win10_21H1_EnglishInternational_x64.iso -o/media/user/UEFI_BOOT/
7z x -i@include.txt Win10_21H1_EnglishInternational_x64.iso -o/media/user/UEFI_BOOT/
7z x -i@exclude.txt Win10_21H1_EnglishInternational_x64.iso -o/media/user/Win10/
```

Done. You're ready to begin your installation

## Credits

This guide is inspired from this article: https://techbit.ca/2019/02/creating-a-bootable-windows-10-uefi-usb-drive-using-linux/ all credits to the original writer.
