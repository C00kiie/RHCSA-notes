<h1> CHAPTER 12: Systemd </h1>

- Systemd is what happens after grub2. It takes control and employs the concept
of (predefined) targets, and units to handle operational states of the
services. 
- systemd includes a service for viewing and managing system logs in addition to the traditionallogging service.
This service maintains a log of runtime activities for fasterretrieval and can be configured to store the 
information permanently.
- Apparently and according to this chapter systemd is a supergroup that also
  includes system tunning to moinitor and optimize other services that tune the
  system / services / units to make it conserve or take more/less performance.


<h3> Chapter 12: System Initialization and Service Management </h3>

- Systemd (system daemon) is such a cool thing because it has a nice startup
  thanks to parallel processing scripts that run the services, and it has
  a nice handling for services dependencies and on demand-activations.
- morever it has the concept of "snapshot" to capture a certain state of the
  system 
- Sysetmd is the first invoked process which has the PID of 1, and it's the
  last process to terminate

- Systemd initiates first by creating a socket for each services, and then
  handle the dependencies by communicating through these sockets. The
  not-needed services don't have to run at all. If any client requests one it's
  the request itself is gonna be cached in the socket. So services like
  printing and scanning are deferred until they are actually needed by the
  user/client
- Another major optimization in systemd is its utilization of 
```bash
**note**: Socket is a communication method that allows a single process to talk to 
another processon the same or remote system.
**another-note**: D-Bus is another communication method that allows multiple services running in parallelon a system to talk to one another on the same or remote system.
```





<h3> Chapter 12: Units! </h3>

- Units are systemd objects. They organize about everything. Mounting and
unmounting things, maintenance tasks or service startups.

- Units have states that are `['active', 'inactive', 'disabled']` and each
  'active' needs to be enabled. Otherwise 'disabled' ones are not allowed to
  run in the first place! - it's much more like `MASKING` a service in `init.d`
- Units have types and they are encoded in files with names in the form
  unitname.type Here are some examples below
    - sshd.services
    - tmp.mount
    - syslog.socket
    - umount.services
  and so on, each task runs in a specific time in a specific way and makes
  everything beautiful and happy!
- There are two kind of systemd units:
  - 1 system based which are are located in /etc/systemd/system
  - 2 user based which are located in /etc/systemd/user
  this heirarchy maintains linux from `ah yes, chaos` 
  - 3 the third and last but not the least kind: temporary units. These are
    only used to temporarily language a unit and delete it. they are located
    the path /run/systemd/system/*.*

In general here what a template looks like:

```bash
[Unit]
Description=OpenBSD Secure Shell server
Documentation=man:sshd(8) man:sshd_config(5)
After=network.target auditd.service
ConditionPathExists=!/etc/ssh/sshd_not_to_be_run

[Service]
EnvironmentFile=-/etc/default/ssh
ExecStartPre=/usr/sbin/sshd -t
ExecStart=/usr/sbin/sshd -D $SSHD_OPTS
ExecReload=/usr/sbin/sshd -t
ExecReload=/bin/kill -HUP $MAINPID
KillMode=process
Restart=on-failure
RestartPreventExitStatus=255
Type=notify
RuntimeDirectory=sshd
RuntimeDirectoryMode=0755

[Install]
WantedBy=multi-user.target
Alias=sshd.service
```

It's pretty straight forward and intuitive to undetstand, too

<h3> Chapter 12: targets </h3>
Targets are simply a collection of units. You can define a target yourself and
it's stored in the same directories of units, except well, their extension is
just "target"

Here's a system standard services:

- halt
- poweroff
- shutdown
- rescue
- emergency
- multi-user
- graphical
- reboot
- default
- hibernate

All are pretty self-explanatory except for default as it is a soft symlink to
to the default target the system boots to 

<h3> CHAPTER 12: SYSTEMCTL </h3>
systemctl is the cli interactive version of systemd 

- you can set the default target the system boots you to by doing
  `systemctl set-default set-default multi-user`

<h3> Chatper 12: System Logging </h3>

System logging is managed by rsyslogd (rocket-fast system for log processing)
which is a multi threaded service with support for enhanced filtering and
encryption-protected emssages. Its configuration files are in /etc/rsyslog.conf
in RHEL8. 

<h3> Chapter 12: Rotating logger  </h3>
- logrotate manages log rotates all the time. its configurations are located in
/etc/logrotate.d/$SERVICE and its configuration are /etc/logrotate.conf
Simply add a new log rotation for any file by just adding a new file that
follows its format in /etc/logrotate.d

- To log certain actions manually you can literally just type "logger -i "LOL
  THIS WILL GET PUT IN SYSLOG, IT HAS NICE INFORMATION TOO", You can now just
  check /etc/messages and you'll notice that. It's pretty handy in scripting
  too

<h3> CHAPTER 12: Journalctl </h3>
Jorunalctl gathers all the messages from everywhere and puts it in a journal
where it can be queried and classified too. 
It gathers messages from kernel and rsyslogd other services

Here are some useful tips to using `journalctl`

- to display detailed output for each entry use -o for verbose
- to view messages since last reboot, use -1, -2 for last-last report, and -n
  for (last$IFS)^n restart report
- to view kernel only messages we can use `journalctl -kb0`
- to view the last n messages, we can do `jornalctl -n3`
- to display messages of a certain services we can just type `joruanlctl
  _SYSTEMD_UNIT=sshd.service`
- you can also use --since and --until to set a certain period for the logged
  messages
- you can print all the last messages by `journalctl -f` similiar to `tail -f`
- **Journalctl by default is non persistent and memory only**: you can change
  that and set it to persistent by creating /var/log/journal manually and
  it'll automatically start to store messages there 

<h3> Tuned Service </h3>

