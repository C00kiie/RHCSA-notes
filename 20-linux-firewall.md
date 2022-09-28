<h1> Chapter 20: Firewall within redhat RHEL </h1>


objectives: 
- Restrict network access using firewall-cmd/firewall
- Configure firewall settings using firewall-cmd/firewalld



```
# install packages
yum install firewall-config

# start firewall
systemctl enable --now firewalld

# get zones
firewall-cmd --get-zones

# get default zone
firewall-cmd --get-default-zone

# list all
firewall-cmd --list-all

# list all for zone
firewall-cmd --zone=home --list-all

# add ip range to zone
firewall-cmd --zone=home --add-source=192.168.10.0/24

# make changes permanent
firewall-cmd --zone=home --add-source=192.168.10.0/24 --permanent

# reload to see changes
firewall-cmd --reload

# add service
firewall-cmd --zone=public --add-port=80/tcp --permanent

# GUI based
firewall-config

# deny all connections
firewall-cmd --panic-on

# to remove a service/custom services
firewall-cmd --permanent --remove-service=$SERVICE_NAME
rm -f /etc/firewalld/services/$SERVICE_NAME.xml*
firewall-cmd --reload

```