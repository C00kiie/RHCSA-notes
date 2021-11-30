<h1> Chapter 6: Advanced User Management </h1>

Objectives:
- log in and switch users in multi-user targets
- locking / unlocking users accounts
- change passwords and adjust password aging for local user accounts
- create , delete, modify local groups and group memberships
- confgigure superuser access
 
<h3> Section 1: Passwords </h3>

Passwords aging is a mechanism to put restrictions on user and allow us to
introduce contorl over users password-change frequency.

```bash
# we can change a user min, max and other attributes by using chage
[root@localhost ~]# chage user4200
Changing the aging information for user4200
Enter the new value, or press ENTER for the default

        Minimum Password Age [100]:
        Maximum Password Age [500]: 1000
        Last Password Change (YYYY-MM-DD) [2021-03-24]:
        Password Expiration Warning [7]: 6
        Password Inactive [-1]:
        Account Expiration Date (YYYY-MM-DD) [-1]: 2022-04-01
[root@localhost ~]#
```
which proves to be extremely useful.

###
<h2> Locking accounts </h2>
the command `passwd` provides all the utiliterian tools that we need to manage
accounts.

```bash
Usage: passwd [OPTION...] <accountName>
  -k, --keep-tokens       keep non-expired authentication tokens
  -d, --delete            delete the password for the named account (root
only); also removes
                          password lock if any
  -l, --lock              lock the password for the named account (root only)
  -u, --unlock            unlock the password for the named account (root only)
  -e, --expire            expire the password for the named account (root only)
  -f, --force             force operation
  -x, --maximum=DAYS      maximum password lifetime (root only)
  -n, --minimum=DAYS      minimum password lifetime (root only)
  -w, --warning=DAYS      number of days warning users receives before password
expiration (root
                          only)
  -i, --inactive=DAYS     number of days after password expiration when an
account becomes disabled
                          (root only)
  -S, --status            report password status on the named account (root
only)
      --stdin             read new tokens from stdin (root only)

Help options:
  -?, --help              Show this help message
      --usage             Display brief usage message

```
We can change about every aspect about a single user. 
- locking, unlocking and modifying expiry parameters
- we can query the status of a certain user with the `--status / -S` command,
  although only root can use it
- it's good to note that a locked account is denoted by an excalamtion mark in
  its row in `/etc/shadow` as shown below

```bash
[root@localhost ~]# passwd --lock user4200
Locking password for user user4200.
passwd: Success
[root@localhost ~]# cat /etc/shadow | grep user4200
user4200:!!$6$kTbEPujI.KuxQgay$mTtdpzAQ8T3MXC8EQH2dM78XOKh5zIH3jEfrhDACQYw8xYw7u1UWkWDa9lTd/dKPxLLM9KLCsAjb4lHGzzVUx1:18710:5:28:5::18658:
[root@localhost ~]# passwd --unlock user4200
Unlocking password for user user4200.
passwd: Success
user4200:$6$kTbEPujI.KuxQgay$mTtdpzAQ8T3MXC8EQH2dM78XOKh5zIH3jEfrhDACQYw8xYw7u1UWkWDa9lTd/dKPxLLM9KLCsAjb4lHGzzVUx1:18710:5:28:5::18658:
[root@localhost ~]#
```

####
<h3> Section 2: Linux Groups And Management </h3>

much more like `useradd / usermod / userdel` we have `groupadd, groupdmod,
groupdel` which take the same exact arguments style respectively, except that
users have a wide range of arguments and here we only a have `gid`, and
`groupname`

- to create a groupd go ahead with `groupadd` and you can specify gid, name and
  non-uniqueness of the said group name
- change a group name or gid use `groupmod`
- finally to delete a group just use `groupdel`

It's that easy!


#####
<h3> Section 3: Linux Sudoers </h3>

Linux sudoers file is a file to manage who can escalate to a superuser or use
a subset of superuser-ship (`/etc/sudoers`)

```bash
%GROUP ALL=(ALL) ALL

USER  ALL=/usr/bin/cat
USER2 ALL=/usr/bin/cat NOPASSWD:ALL
```
we can add groups or users that can perform sudo, select some or all commands,
with and without password too.

we can also cluster commands inside it. For example

```bash
AUTHORIZED_TOOLS  PKGCMD = /usr/bin/cat, /usr/bin/yum
AUTHORIZED_USERS  PKGADM = user1, user100, user200
PKGADM ALL = PKGCMD
```
which is roughly the same as what we do with groups except here we get to
specify the tools as well.


