# Install Windows Using Dism and Command Line

## Bootable USB

Prepare for bootable USB with some tools to create partitions, disk management.

## Diskpart: Partition Preparation

Once booted into bootable system (say Windows 10 XPE), we can launch diskpart to create partition.

1. Start CMD and initiate diskpart

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

2. Next, we will create a partition for Windows.

```sh
#Create a primary partition with size 100GB = 102400MB
create part primary size 102400
format quick label Windows
assign letter C
```

3. You can create addition partition for your data.

```sh
create part primary
format quick label Data
assign letter D
```

## Unpack Windows from WIM file.

4. Now, it is the time to install your Windows from WIM file.

Suppose your Windows installation image is located at E:\sources\install.wim

```sh
dism /get-wiminfo /wimfile:E:\sources\install.wim
```

From the above command, you can see the index of all Windows SKUs that you want to deploy

```sh
dism /apply-image /imagefile:E:\sources\install.wim /index:2 /applydir:C:\
```

5. Activate your UEFI boot

```sh
bootsect C:\Windows /s W:
```

6. Reboot to enjoy your new system.

