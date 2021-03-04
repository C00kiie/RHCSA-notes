<h1> Chapter </h1>


<h3> Setting up the lab </h3>
**This chapter includes basic lab configuraition which consists of**:

- Vm1: 192.168.0.110 / 1 CPU / 1GB RAM /  1 x 10 GB OS disk
- Vm2 192.168.0.120 / 1 CPU / 2GB ram / 4 x 250 MB Disk / 1 x 4GB disk / 2 x 1GB Disk /1 x 10 GB os disk

Creating this lab is extremely easy. **steps**:

- Create VM1 
- Create VM2
- Create a vlan inside virtualbox / vmware with these given parameters: 192.168.0.0/24, 255.255.255.0, DHCP is not recommended because we aren't using it to begin with 
- Connect both to the vlan
- Now make the IPs static by adjusting files under **/etc/scripts/network-manager/** according to this article:  https://cloudcone.com/docs/article/how-to-configure-a-static-ip-address-on-linux/

You'll end up with a config file that adjusts IP on startup or reset of network manager service 

config file:
 
```bash
HWADDR=08:00:27:1d:2a:e1 
TYPE=Ethernet
BOOTPROTO=none
# Server IP #
IPADDR=192.168.0.110 # or 120 for the other machine
# Subnet #
NETMASK=255.255.255.0
# Set default gateway IP #
GATEWAY=192.168.0.1
# Set dns servers #
DNS1=192.168.0.1
DNS2=8.8.8.8
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
# Disable ipv6 #
IPV6INIT=no
NAME=enp0s3
UUID=41171a6f-bce1-44de-8a6e-cf5e782f8bd6
DEVICE=eth0
ONBOOT=yes
```

<h3> Virtual Consoles </h3>
To access any kind of virtual Console just hit **Ctrl+Alt+Fx** where x is the virtual console id.
