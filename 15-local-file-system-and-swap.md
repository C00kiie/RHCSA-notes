<h1> Chapter 15 </h1>
Local file systems and swap manipulation



### ext4 journaling system and metadata (and inodes per se)
The metadata includes the superblock, which keeps vital file system
structural information, such as the type, size, and status of the file system,
and the number of data blocks it contains. Since the superblock holds such
critical information, it is automatically replicated and maintained at various
known locations throughout the file system. The superblock at the beginning
of the file system is referred to as the primary superblock, and all of its
copies as backup superblocks. If the primary superblock is corrupted or lost,
it renders the file system inaccessible. One of the backup superblocks is then
used to supplant the corrupted or lost primary superblock to bring the file
system back to its normal state.
The metadata also contains the inode table, which maintains a list of index
node (inode) numbers. Each file is assigned an inode number at the time of
its creation, and the inode number holds the file’s attributes such as its type,
permissions, ownership, owning group, size, and last access/modification
time. The inode also holds and keeps track of the pointers to the actual data
blocks where the file contents are located
#
the Ext3 and Ext4 file systems support a journaling mechanism that
provides them with the ability to recover swiftly after a system crash. Both
Ext3 and Ext4 file systems keep track of recent changes in their metadata in
a journal (or log). Each metadata update is written in its entirety to the
journal after completion. The system peruses the journal of each extended
file system following the reboot after a crash to determine if there are any
errors, and it recovers the file system rapidly using the latest metadata
information stored in its journal. The ext2 file system does not support
journaling, but the support for journaling may be added to it if required.
In contrast to Ext3 that supports file systems up to 16TiB and files up to
2TiB, Ext4 supports very large file systems up to 1EiB (ExbiByte) and files
up to 16TiB (TebiByte). Additionally, Ext4 uses a series of contiguous
physical blocks on the hard disk called extents, resulting in improved read
and write performance with reduced fragmentation. Ext4 supports extended


### XFS File System
The X File System (XFS) is a high-performing 64-bit extent-based journaling
file system type. XFS allows the creation of file systems and files up to 8EiB
(ExbiByte). It does not run file system checks at system boot; rather, it relies
on you to use the xfs_repair utility to manually fix any issues. XFS sets the
extended user attributes and acl mount options by default on new file
systems. It enables defragmentation on mounted and active file systems to
keep as much data in contiguous blocks as possible for faster access. The
only major caveat with using XFS is its inability to shrink



### File System Management
Managing file systems involves such operations as creating, mounting,
labeling, viewing, growing, shrinking, unmounting


### general guide
- `mount -t $TYPE` can be used to show file systems of x type
- `/proc/self/mounts` used by kernel to keep track of all mounts on the system
- use `man mount` to see all the mounting options within mount. It's all well documented
- on `xfs` you can view system UUID labels by `xfs_admin -u $DRIVE_NAME` 
- alternatively you can use `lsblk -f $DRIVE` where `DRIVE` is a logical disk like `/dev/sda1`
-  another property within XFS (AFAIK) is labels, you can set a custom label instead of using UUIDs
  - `xfs_admin -l $logical_drive` would output the drive name
  - `xfs_admin -L "drive_new_label" $logical_drive` would set the label, for example
    `xfs_admin -L "bootfs" /dev/sda1` 
  - Now you can replace the UUID="c1ff315e-4320-442c-a3c5-36db403b53f2"
    for /boot in the fstab file with LABEL=bootfs, and unmount and remount
    /boot as demonstrated above for confirmation
  - it's not recommended to use labels in `/etc/fstab`




### Practice !

- make an ext4 disk, format it, and mount it
- make an xfs disk, format it, and mount it
  