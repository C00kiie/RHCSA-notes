<h1> Chapter 14: Advanced Storage Paritioning </h1>

this chapter talks about LVM (physical volumes, volume groups and logical volumes), it also introduces
us to Stratis


<h2> LVM Volumes </h2>

- physical volumes: they make the very first blocks of the hierarchy, they are the actual HDDs/SSDs that 
  store the data. 
  - First initialize a new physical volume, we have to first make a parition and mark it as LVM
  - Secondly, run `pvcreate /dev/DEVICE_NAME` 
  - and that's it
- volume groups: volume groups are a bunch of physical volumes that are together. This way we can combine 
  multiple disk units into one group that fits nicely
  - to create a volume group it's as simple as: `vgcreate VGROUP_NAME /dev/DEVICE_A /dev/DEVICE_B`
- logical volumes: logical volumes are the interface we have to to map our volume groups as devices 
  that can be formatted and utilized. 
  - create a logical volume, simply proceed with `lvcreate -n "lvm_volume_name" -L 10G VGROUP_NAME`


note that:
- to add another volume to a group, we can use `pvextend`, and we can also remove others with `pvreduce`
- we can also reduce logical volumes by `lvreduce -L NEW_SIZE /dev/mapper/vgrp0-first-vol`


<h2> Stratis </h2>
