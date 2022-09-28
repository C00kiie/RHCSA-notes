<h1> Chapter 18: time synchronization and hostname resolution </h1>

The goal of this chapter is to make able to do the following:
- understand stratums level [1]
- install and configure NTP Client
- dns configuration `/etc/resolv.conf`


### stratum levels
efore learning about stratum levels, it is important to understand the term Network Time Protocol (NTP). An NTP is a protocol that is used to synchronize the clocks of the computer to some time reference. NTP helps network engineers synchronize time across multiple network devices. Now, the question is, why there's a need for synchronization? If the network devices are out of sync by even a few milliseconds, it can be very difficult for network administrators to determine the sequence of events taking place within the network. Time synchronization is a crucial part of monitoring a network and resolving issues within it. NTP allows network devices to synchronize their clocks with a central source clock. An NTP network usually gets its time from an authoritative time source, such as an atomic clock or a radio clock attached to a time server. NTP then distributes this time across the network.

In Network Time Protocol, time is distributed from a high precision time device such as radio clock or atomic clocks, hierarchically, with the primary servers at the top keeping the master time, and distributing the time down to the secondary servers and so on. Stratum is defined as the distance from an authoritative time source to the network devices. 

Each level of this hierarchy is assigned a number, starting with zero (0) for the reference clock at the top. Stratum 0 means that a device is directly connected to the atomic clock e.g., a GPS antenna. Stratum 1 devices have a direct connection with the primary time source, and they provide that time to clients on stratum 2 via a network connection. Stratum 2 devices can function like a server by providing time to clients on stratum 3, and Stratum 3 can provide it to those on stratum 4, and so on. A total of 15 synchronized stratum levels are possible, but each one introduces another layer of network delay, causing accuracy to decrease.


### installing and configuring NTP Client

we use chrony for an ntp client. To do that follow the following steps

# installation

- install chrony with dnf: `dnf install -y chrony`
- edit the configuration file located in `/etc/chrony.conf` and adjust the servers 
- make sure to persist chrony service `systemctl enable chrony` 

# verification
use these commands to verify the situation
- `chronyc sources`
- `chronyc tracking`
- `timedatectl`

# turning NTP sync on or off

- to turn it on `timedatectl set-ntp true`
- turn it off `timedatectl set-ntp false`
- setting can be checked by `timedatectl | grep -i ntp` which will display:

```
              NTP service: active
```

### DNS Settings
adjust the file `/etc/resolv.conf` to contain all the name servers you may require

example:
```
1.1.1.1
8.8.8.8
```


# changing time manually
use `timedatectl set-time "2019-06-21 12:00`


### refs
[1] https://www.everythingrf.com/community/understanding-stratum-levels