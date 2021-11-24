<h1> Chapter 13: Basic Storage Partitioning </h1>

This chapter goes through the basics of partitioning a simple GPT / MBR table 
along with partioning other types of storage (ext4, zfs and swap and what not).
The second objective is the configuration of disk compression.

the key in this chapter is consistency and remembering the most prominent commands.

<h3> Section 1: GPT/MBR Disks Partitioning </h3>

<h4> tips & tools</h4>
RHEL offers numerous tools and toolsets for storage management, and they
include parted, gdisk, VDO, LVM, and Stratis. There are other native tools
available in the OS, but their discussion is beyond the scope of this book.
Partitions created with a combination of most of these tools and toolsets can
coexist on the same disk. We look at parted, gdisk, and VDO in this chapter and
LVM and Stratis in Chapter 14 “Advanced Storage Partitioning”.
parted is a simple tool that understands both MBR and GPT formats. gdisk is
designed to support the GPT format only, and it may be used as a replacement of
parted. VDO is a disk optimizer software that takes advantage of certain
technologies to minimize the overall data footprint on storage devices. LVM is a
feature-rich logical volume management solution that gives flexibility in storage

- `fdisk -l` can be used to list the available disks/partitions
- parted can understand both MBR and gpt, while gdisk / gparted only use GPT which can be used as a replacement

<h4>MBR </h4>
MBR allows the creation of three types of partition—primary, extended, and
logical—on a single disk. Of these, only primary and logical can be used for
data storage; the extended is a mere enclosure for holding the logical partitions
and it is not meant for data storage. MBR supports the creation of up to four
primary partitions numbered 1 through 4 at a time. In case additional partitions
are required, one of the primary partitions must be deleted and replaced with an
extended partition to be able to add logical partitions (up to 11) within that
extended partition. Numbering for logical partitions begins at 5. MBR supports a
maximum of 14 usable partitions (3 primary and 11 logical) on a single disk.

<h4> GPT / GUID </h4>
With the increasing use of disks larger than 2TB on x86 computers, a new 64-bit
partitioning standard called Globally Unique Identifiers (GUID) Partition Table
(GPT) was developed and integrated into the UEFI firmware. This new standard
introduced plenty of enhancements, including the ability to construct up to 128
partitions (no concept of extended or logical partitions), utilize disks larger than
2TB, use 4KB sector size, and store a copy of the partition information before
the end of the disk for redundancy.

<h4> What's Thin Provisioning</h4>
Thin provisioning technology allows for an economical allocation and utilization
of storage space by moving arbitrary data blocks to contiguous locations, which
results in empty block elimination. With thin provisioning support in VDO,
LVM, and Stratis, you can create a thin pool of storage space and assign
volumes much larger storage space than the physical capacity of the pool.
Workloads begin consuming the actual allocated space for data writing. When a
preset custom threshold (80%, for instance) on the actual consumption of the
physical storage in the pool is reached, expand the pool dynamically by adding
more physical storage to it. The volumes will automatically start exploiting the
new space right away. The thin provisioning technique helps prevent spending
more money upfront.

<h3> Section 2: LABS </h3>

lab objectives:
- List your disk partitions using various tools
- Create a new partition and put a file system on it
- Unmount the file system
- Create an extended and logical partitions and confirm

Solution steps:
```bash
fdisk -l # lists partitions

fdisk /dev/xvdd
n # new partition
p # type primary
enter 
+1G # size of 1Gigabyte
w # write to Disk

fdisk /dev/xvdd
n # new partition
e # type extended
enter 
+100M # size of 100 MB
w # write to Disk

```

the results are: 
```bash
Disk /dev/xvda: 20 GiB, 21474836480 bytes, 41943040 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0xdd4d900e

Device     Boot Start      End  Sectors Size Id Type
/dev/xvda1       2048     4095     2048   1M 83 Linux
/dev/xvda2 *     4096 41943006 41938911  20G 83 Linux


Disk /dev/xvdd: 2 GiB, 2147483648 bytes, 4194304 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x90c14272

Device     Boot   Start     End Sectors  Size Id Type
/dev/xvdd1         2048 2099199 2097152    1G 83 Linux
/dev/xvdd2      2099200 2303999  204800  100M  5 Extended


Disk /dev/xvdb: 2 GiB, 2147483648 bytes, 4194304 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/xvdc: 2 GiB, 2147483648 bytes, 4194304 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
```

2 - logical volumes
to create a logical volume, you have to have an extended volume first. Think of it as a placeholder for other logical volumes
to set on top of it.
```bash
Command (m for help): n                                                                       
Partition type                                                                                
   p   primary (0 primary, 0 extended, 4 free)                                                
   e   extended (container for logical partitions)                                            
Select (default p): e                                                                         
Partition number (1-4, default 1):                                                            
First sector (2048-4194303, default 2048):                                                    
Last sector, +sectors or +size{K,M,G,T,P} (2048-4194303, default 4194303):                    
                                                                                              
Created a new partition 1 of type 'Extended' and of size 2 GiB.                               
                                                                                              
Command (m for help): n                                                                       
All space for primary partitions is in use.                                                   
Adding logical partition 5                                                                    
First sector (4096-4194303, default 4096):                                                    
Last sector, +sectors or +size{K,M,G,T,P} (4096-4194303, default 4194303): +1G                
                                                                                              
Created a new partition 5 of type 'Linux' and of size 1 GiB.                                  
                                                                                              
Command (m for help): n                                                                       
All space for primary partitions is in use.                                                   
Adding logical partition 6                                                                    
First sector (2103296-4194303, default 2103296):                                              
Last sector, +sectors or +size{K,M,G,T,P} (2103296-4194303, default 4194303):                 
                                                                                              
Created a new partition 6 of type 'Linux' and of size 1021 MiB.                               
                                                                                              
Command (m for help): w                                                                       
The partition table has been altered.                                                         
Calling ioctl() to re-read partition table.                                                   
Syncing disks.                                
```

3 - making the system automatically mount a disk

```bash
lsblk -f
NAME    FSTYPE LABEL UUID                                 MOUNTPOINT
xvda                                                      
├─xvda1                                                   
└─xvda2 xfs          58013e4a-11c0-4195-8fd8-e4b33e5b17d6 /
xvdb                                                      
xvdc                                                      
├─xvdc1                                                   
├─xvdc5                                                   
└─xvdc6                                                   
xvdd                                                      
├─xvdd1 ext4         a1a62bce-2ffd-4d61-8fc4-23b9a225bb5c /mnt/ext4mountpoint
└─xvdd2               

# using the UUID, we insert this new line in /etc/fstab:
# UUID=a1a62bce-2ffd-4d61-8fc4-23b9a225bb5c /mnt/ext4mountpoint     ext4    defaults        0 0
# /mnt/ext4mountpoint is the mount point, obviously, the rest is just defaults.
# after doing that you can make sure everything is fine if mount -a doesn't return any errors
mount -a
mount | grep /dev/xvdd 
/dev/xvdd1 on /mnt/ext4mountpoint type ext4 (rw,relatime,seclabel)
```
4 - making an extra swap for our linux system

```bash
mkswap /dev/xvdb
Setting up swapspace version 1, size = 2 GiB (2147479552 bytes)
no label, UUID=78a7b92b-6a1f-4dd0-940e-3cf45702c950
# add this label now to /etc/fstab, to make sure it's persistent.

NAME    FSTYPE LABEL UUID                                 MOUNTPOINT
xvda                                                      
├─xvda1                                                   
└─xvda2 xfs          58013e4a-11c0-4195-8fd8-e4b33e5b17d6 /
xvdb    swap         78a7b92b-6a1f-4dd0-940e-3cf45702c950 
xvdc                                                      
├─xvdc1                                                   
├─xvdc5                                                   
└─xvdc6                                                   
xvdd                                                      
├─xvdd1 ext4         a1a62bce-2ffd-4d61-8fc4-23b9a225bb5c /mnt/ext4mountpoint
└─xvdd2                                                   
root@server1: ~ # free -h
              total        used        free      shared  buff/cache   available
Mem:          3.6Gi       339Mi       2.5Gi        18Mi       815Mi       3.1Gi
Swap:         8.0Gi          0B       8.0Gi
root@server1: ~ # swapon /dev/xvdb -v
swapon: /dev/xvdb: found signature [pagesize=4096, signature=swap]
swapon: /dev/xvdb: pagesize=4096, swapsize=2147483648, devsize=2147483648
swapon /dev/xvdb
root@server1: ~ # free -h
              total        used        free      shared  buff/cache   available
Mem:          3.6Gi       341Mi       2.5Gi        18Mi       815Mi       3.1Gi
Swap:           9Gi          0B         9Gi
root@server1: ~ # 
```
5 - making more swap with files
this is generally the same, except we have to make a file with `dd` that contains `/dev/zero` characters.

```bash
dd if=/dev/zero of=/moreswaplz bs=1024 count=1024000                                                                                                                       
1024000+0 records in
1024000+0 records out
1048576000 bytes (1.0 GB, 1000 MiB) copied, 3.80502 s, 276 MB/s
root@server1: ~ # mkswap /moreswaplz
mkswap: /moreswaplz: insecure permissions 0644, 0600 suggested.
Setting up swapspace version 1, size = 1000 MiB (1048571904 bytes)
no label, UUID=faa94e96-7c93-4b54-a4a8-dd0e2ea83e6a
root@server1: ~ # nano /etc/fstab
root@server1: ~ # vi /etc/fstab # add it as /moreswap instead of using UUID=...
root@server1: ~ # swapon -v /moreswaplz
swapon: /moreswaplz: insecure permissions 0644, 0600 suggested.
swapon: /moreswaplz: found signature [pagesize=4096, signature=swap]
swapon: /moreswaplz: pagesize=4096, swapsize=1048576000, devsize=1048576000
swapon /moreswaplz
root@server1: ~ # chmod 600 /moreswaplz
root@server1: ~ # free -h
              total        used        free      shared  buff/cache   available
Mem:          3.6Gi       338Mi       1.5Gi        18Mi       1.8Gi       3.1Gi
Swap:          10Gi          0B        10Gi
root@server1: ~ # 
```




