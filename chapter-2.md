<h1> Chapter 2 </h1>


<h3> Graphical interface </h3>
this chapter goes into showing you the UI and main directives and what not. I'm not interested in explaining them here because it's as easy as exploring it by yourself. The book goes into details about navigating the UI.

<h3> Linux Directory Strucuture and File Systems </h3>
Linux files are organized logically in a hierarchy for ease of administration and
recognition. This organization is maintained in hundreds of directories located in
larger containers called file systems. Red Hat Enterprise Linux follows the
Filesystem Hierarchy Standard (FHS) for file organization

it's worth the while to mention that linux files strucutre is exactly like an inverted tree. Splits form node-0 (Which is /) into multi nodes 
and each parent has a child and what not

**Here are the important things to rememeber about files strucuture**:

```bash
/ root 
|
-> /dev
|| --> /block
   --> /pts
|
-> /etc
|| --> /skel
   --> /systemd
   --> /opt
   --> /sysconfig
   --> /cron.d
   --> /lvm
|
|
-> /sys
|
|| --> /kernel
   --> /fs
|
-> /home
|
-> /opt
|
-> /root
|
-> /tmp
|
-> /usr
|| --> /lib
   --> /lib64
   --> /share
   --> /sbin
   --> /bin
   --> /local
|
-> /boot
|| --> /grub2
|
-> /var
||
 --> /log
 --> /mail
 --> /tmp
 --> /spool
 --> /opt
```

**Here is a visual implementation**: 
[linux-files!](img/chapter_2/linux-files-tree.png)


- There are three kinds of files:
 - disk based: images, text files .. etc 
 - network based: disk based files but somewhere not in our physical disks
 - memory based: much more like /proc and its contents, it's all in your mind

---
<h3> Disk Based Examples <h3>

- / (root) 
- /etc
- /mnt (Used to mount other disks
- /boot used to store linux boot loader which loads the kernel
- /home land, self explanatory 
- /usr this folder stores some executables under `/usr/bin` and some other `/usr/sbin`. The latter includes at-boot-run-time executable
  as well as binaries that require administrator/root permissions. It also includes `/usr/lib` which stores system initlization data and service mangament.
  it also includes `/usr/lib64` which stores x86-x64 shared-libraries routines.
  it  also includes `/usr/include` which includes c header files 
  it also includes `/usr/share` which includes manual pages, documentation, sample templates, configuration files ... etc
  it also includes `/usr/src` which stores source code for various software if any

- /var this includes plenty of important important parts of linux 
 - `/var/log`
 - `/var/www`
 - `/var/tmp`: a temp directory that stores files for a longer period than `/tmp`. The first stores it for a period of 30 days if not accessed, while the latter only stores files for 10 days.
 - `/var/opt`: the right convenetion to store log files of software located at /opt
 - `/var/spool`: Directories that hold print jobs, cron jobs, mail messages, and other
    queued items before being sent out to their intended destinations are located here

---
<h3> Memory Based Examples </h3>
 
 - ``/dev`` : is used to store device nodes and physical/virtual devices. It's important to note that devices are either block (buffer) devices or character    (single characters) devices.
 
   Block devices are accessed in a parallel method while the other is accessed in a serial stream of bits
 - ``/proc`` : the procs (processes file system) maintaines information about every process and the current state of the kernel. The information is stored in a files-like hierarchy of subdirectories
   that contain thousands of zero-length pseduo files. These files point to relevant data and maintained by the kernel in the memory. You can think of `procfs` as just an interface to the in-memory
   information which provides an easy interaction with the kernel maintained information
 
 - ``/run`` : This virtual system is a repoistory of data for any process that wishes to store run-time data. It's erased at shutdown 
 
 - ``/sys`` : Provides information about hardware devices, drivers, and some kernel features is stored and maintained. The difference between sys and dev, is that sys handles the creation and deletion 
    and status of said device inside /dev, while the other provides an interface to deal with said devices. Most files inside /sys are symbolic linkes and regular files too. Most files in /dev are 


