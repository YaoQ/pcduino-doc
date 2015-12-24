# How to edit pcDuino image file
Maybe you create a pcDuino image file to backup the whole system using dd command, after then you find a bug and have to edit the image file, how can you manage it?
 
You can't mount the image as a whole because it actually contains two partitions and a boot sector. However, you can mount the individual partitions in the image if you know their offset inside the file. To find them, examine the image as a block device with `fdisk -l whatever.img`. The output should include a table like this:

>Device         Boot     Start       End  Blocks  Id System

>whatever.img1            8192    122879   57344   c W95 FAT32 (LBA)

>whatever.img2          122880   5785599 2831360  83 Linux

These are the two partitions. The first one is labelled "FAT32", and the other one "Linux". Above this table, there's some other information about the device as a whole, including:

**Units: sectors of 1 * 512 = 512 bytes**

We can find the offset in bytes by multiplying this unit size by the Start block of the partition:

> 1st partition 512 * 8192 = 4194304

> 2nd partition 512 * 122880 = 62914560

These can be used with the offset option of the mount command. We also have a clue about the type of each partition from fdisk. So, presuming we have directories /mnt/img/one and /mnt/img/two available as mount points:

```bash
mount -v -o offset=4194304 -t vfat whatever.img /mnt/img/one
mount -v -o offset=62914560 -t ext4 whatever.img /mnt/img/two
```
Note:If you have a error when you mount the image file, please try **-t auto** option instead of **-t ext4**

You can now access the two partitions. If you do not intend to change anything in them, use the **-r** (read-only) switch too. If you do change anything, those changes will be included in the .img file.

**Note that the first partition is probably mounted on /boot in the second partition when the system is running.**
