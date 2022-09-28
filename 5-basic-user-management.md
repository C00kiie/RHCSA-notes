<h1> Chapter 5: Basic User Management  </h1>

Objectives
- create, delete and modify local user accounts
- change passwords and adjust password aging for local user accounts


Honestly these things couldn't get eaiser. Linux is plain and simple and gives us the tools to do all these things in order without any problems 


#####

<h3> Section A: User </h3>

- `Who` and `w` shows which users logged in but the latter displays for how long it has been logged in
- `last` shows an extensive history of users login on the system with which kernel they have logged in with and and at which date/time 
- `lastb` reads from a file /var/log/btmp which shows last failed attempts to login 
- `lastlog` shows when did a user login last time. Interesting tool!
- `newgrp` can be used to change a user group temporarily if and only if a group has a password. Cool! (check the /etc/group)
 - `gpasswd` can be used to change a group password, it stores passwords either in `/etc/group` or `/etc/gshadow` much more like `/etc/passwd` and `/etc/shadow`


#####

<h2> Adding, Removing and updating on users </h2>

- `useradd` adds users without a question based on defaults placed in `/etc/login.defs`. It's a good idea to edit them if needed!
 - some useful arguments are (m) which creates a home directory if it doesn't exist; b specifies where put the base folder (by default /home)
   o makes a none unqiue uid so it can copies off another user uid; u can be used to specify uid too; a means append to group
   s can be used to specify shell; g/gid can be used to specify primary group and supplemntary groups.
- `userdel` on the opposite side deletes a user account

- `usermod` can be used to modify a user group for example adding a user to a group `usermod -aG sudo USERNAME`. Usermod uses the same options
 note that it uses the same options as useradd.


Note: You can create a non login account by `useradd -s /sbin/nologin USERNAME` which effectively starts `USERNAME` with `nologin` shell.



