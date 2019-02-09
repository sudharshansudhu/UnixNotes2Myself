# Description: Hard Disk Administration on Ubuntu Using fdisk

## Install a New Hard Drive on Ubuntu
### Determine Hard Drive Information

```shell
sudo lshw -C disk

# Note the "logical name" entry from the output
*-disk
       description: ATA Disk
       product: IC25N040ATCS04-0
       vendor: Hitachi
       physical id: 0
       bus info: ide@0.0
       logical name: /dev/sdb
       version: CA4OA71A
       serial: CSH405DCLSHK6B
       size: 37GB
       capacity: 37GB
       
# Alternate way to find the size of the current disk (If logical name is known)
sudo fdisk -l /dev/sdb

# Sample output will be as follows
Disk /dev/sdb: 4000.8 GB, 4000787030016 bytes
255 heads, 63 sectors/track, 486401 cylinders, total 7814037168 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 4096 bytes
I/O size (minimum/optimal): 4096 bytes / 4096 bytes
Disk identifier: 0x00000000
```

### Partition The Disk using fdisk
- GUI tool GParted or Termnal tool parted are other aternatives to fdisk. 

```shell
# The /dev/sdb is the logical name obtaining the previous step.
sudo fdisk /dev/sdb

# Type 'm' to see all the options.

# Type 'n' to add a new partition.

# Type 'p' for primary partition.

# Press ENTER to keep the default value for 'Partition Type'.

# Again press ENTER to keep the default value of 1 for 'Partition Number'.

# Again press ENTER to keep the default value of 2048 for 'First Sector'.

# Again press ENTER to keep the default value for 'Last Sector'.

# Type 'w' to write the partition table to the disk.

# It will display a message "The partition table has been altered!".
```

### Format the new partition as ext4 file system.
```shell
sudo mkfs -t ext4 /dev/sdb1
sudo mkfs.ext4 /dev/sdb1   # Alternative
```

### Create A Mount Point
```shell
sudo mkdir /media/harddisk_2
```

### Add Automatic Mount At Boot
```shell
sudo vim /etc/fstab

# Add the following entry to the table 
/dev/sdb1    /media/harddisk_2   ext4    defaults     0        1
```

### Mount or Restart the machine
- Either reboot the computer to have the changes take effect or execute the following command.
```shell
sudo mount -a
```

### Confirm Hard Disk Setup After Restarting Machine
```shell
sudo lsblk -o NAME,FSTYPE,SIZE,MOUNTPOINT,LABEL
```

### TODO
* None
