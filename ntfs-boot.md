# NTFS UEFI BOOT


Preparing a portable HDD or USB to be loaded using efi and supporting NTFS has many beneficial that we can exploit. We can use Rufus to get this task done easily. However, if you want to create it by yourself, we can follow the following steps:

1) Create an EFI (FAT32) partition

```sh
diskpart
list disk
# Use the above information to select the right disk
select disk 0
# If you want to clean all partitions, make sure you back up your data.
clean # This willl wipe out your data.
# If the disk is not in gpt, you can convert to gpt
convert gpt
# Create EFI partition with fat32
create part efi size 256
format fs fat32 quick
assign letter w
```

After this step, we need to copy two files bootia32.efi and bootx64.efi to EFI\BOOT in W drive.

2) Create an NTFS partition to store the main system


```sh
#Create a primary partition
create part primary
format quick label BOOTDATA
assign letter Y
```

Copy the bootable sources including efi\boot\bootx64.efi from the ISO windows.

3) Understand the loading process:

[FAT32]\EFI\BOOT\bootx64.efi starts up the boot process then it will load the actual bootloader [BOOTDATA]\EFI\BOOT\bootx64.efi to intiate windows system.

