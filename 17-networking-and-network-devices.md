<h1> Chapter 17: Networking, Network Devices and network-connections </h1>


the only thing that is important in this chapter is to define static IPs and new
itnerfaces when needed.

All the necessary files lie with `/etc/sysconfig/network-scripts/ifcfg-$INTERFACE_NAME`

# static IP

```
DEVICE="eth0"
ONBOOT=yes
NETBOOT=yes
UUID="41171a6f-bce1-44de-8a6e-cf5e782f8bd6"
IPV6INIT=yes
BOOTPROTO=dhcp
HWADDR="00:08:a2:0a:ba:b8"
TYPE=Ethernet
NAME="eth0"
```
onces these configuration files are written,

```
systemctl restart network
```

# verify
- `ip addr`
- `ifconfig`

# troubleshooting
verify your syntax as it can be very hard to memorize it or repeat it