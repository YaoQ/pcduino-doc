# 如何手动扩展pcDuino的磁盘分区
有时候，我们将8G的镜像写到16G的SD卡中，系统只能够识别8G，这个时候我们需要手动扩展分区大小，使系统能够识别整个SD卡。
具体步骤如下：
### 分区
```bash
linaro@linaro-alip:~$ df -h
文件系统        容量  已用  可用 已用% 挂载点
/dev/mmcblk0p3  7.3G  6.2G  823M   89% /
none            437M  4.0K  437M    1% /dev
none            4.0K     0  4.0K    0% /sys/fs/cgroup
none            437M  4.0K  437M    1% /tmp
none             88M  2.7M   85M    3% /run
none            437M     0  437M    0% /var/tmp
none            437M  528K  437M    1% /var/log
none            5.0M     0  5.0M    0% /run/lock
none            437M     0  437M    0% /run/shm
none            100M     0  100M    0% /run/user

linaro@linaro-alip:~$ cat /sys/block/mmcblk0/mmcblk0p3/start
139264（记下这个数）

linaro@linaro-alip:~$ sudo fdisk /dev/mmcblk0
命令(输入 m 获取帮助)： d
分区号 (1-4): 3

命令(输入 m 获取帮助)： n
Partition type:
   p   primary (2 primary, 0 extended, 2 free)
   e   extended
Select (default p): p
分区号 (1-4，默认为 3)： 3
起始 sector (139264-31211519，默认为 139264)： （回车！）
将使用默认值 139264

Last sector, +扇区 or +size{K,M,G} (139264-31211519，默认为 31211519)： （回车！）
将使用默认值 31211519

命令(输入 m 获取帮助)： w
The partition table has been altered!

Calling ioctl() to re-read partition table.

WARNING: Re-reading the partition table failed with error 16: 设备或资源忙.
The kernel still uses the old table. The new table will be used at
the next reboot or after you run partprobe(8) or kpartx(8)
Syncing disks.

linaro@linaro-alip:~$ sudo reboot
```
### 使用resize2fs扩展分区大小
```bash
linaro@linaro-alip:~$ sudo resize2fs /dev/mmcblk0p3
resize2fs 1.42.9 (4-Feb-2014)
Filesystem at /dev/mmcblk0p3 is mounted on /; on-line resizing required
old_desc_blocks = 1, new_desc_blocks = 1
The filesystem on /dev/mmcblk0p3 is now 3884032 blocks long.
```
### 重新查看分区大小
```
linaro@linaro-alip:~$ df -h
文件系统        容量  已用  可用 已用% 挂载点
/dev/mmcblk0p3   15G  6.2G  7.9G   45% /
none            437M  4.0K  437M    1% /dev
none            4.0K     0  4.0K    0% /sys/fs/cgroup
none            437M  4.0K  437M    1% /tmp
none             88M  2.7M   85M    3% /run
none            437M     0  437M    0% /var/tmp
none            437M  528K  437M    1% /var/log
none            5.0M     0  5.0M    0% /run/lock
none            437M     0  437M    0% /run/shm
none            100M     0  100M    0% /run/user
```