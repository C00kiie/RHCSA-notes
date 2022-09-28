<h1> Chapter 21: Selinux </h1>

# SELinux Modes

passive mode: log when the incident happen, but don't enforce it
enforcing mode: log when the incident happen, but AND enforce it
disabled mode: don't do anything, obviously.



### setting up se linux mode


# get the current satate of selinux

`getenforce`

# set permissive mode
`setenforce 0`

# set enforce mode
`setenforce 1`

# see config file
`nano /etc/selinux/config`


note that the above commands will **NOT** persist the settings.
adjust the configuration file in `/etc/selinux/config` to have permanent settings

### relabling system
this can be useful when you change something that isn't allowed by a policy, say
for example changing root password in rescue mode.

`touch /.autorelabel`

which will force the system relabel files **AFTER** reboot