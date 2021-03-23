<h1> Chapter Four: Advanced Files Management </h1>
Objectives:

- List, set, and change standard ugo/rwx permissions
- Create and configure set-GID directories for collaboration
- Diagnose and correct file permission problems
- Create and use file access control lists


<h3> Section 1: Permissions </h3>

Permissions are an abstraction to construct confidentiality in linux systems so
that only unauthorized users can't access other users files, by default once
a user creates a file it's initiated with a full 777 and then the umask
subtracts the umask from the intial value so it's reduced to something lower
than the 777 (rwx) for everyone


Permissions in linux work in octal system and here is the full list

- 1 execute
- 4 read
- 2 write

and only 6 combinations exist to get all states ( 3 checkboxes with
2 possiblities)


```
A B C T
0 0 1 1 
0 1 1 3
0 1 0 2
1 0 0 4 
1 1 1 7
1 0 1 5
```

Which can be summarized as:

0 - none of the permissions

1 - execute only

2 - write only

3 - write and execute

4 - read only

5 - read and execute

6 - read and write

7 - read and write and execute

###

<h2> here's a good umask for you </h2>

- A. 022 anyone can only read on the file (766 - 022 = 744)

- B. 077 no one can read or see the file existence (700)  

- C. 002 anyone in the same group can as much as the original user with the
  said file, except for foreign users they can only read

##########


<h3> Section 2: Sticky bits </h3>

setuids / setgids makes the user who runs the said file run using the
permissions of the owner / group instead of the binary caller permissions.

- setgids is set by `chmod 2000` and can be used on files and directories
- setuids is set by `chmod 4000` and can be sued only on files

<h3> Using find command </h3>

Find command is as simple as it gets and can be super useful for lots of
reasons

```bash
find PATH -arg1 argvalue1 -arg2 argvalue2 ..
```

here are my favorite arguments

```bash

# execute on files that you want to find. the `-ok` is optional but it allows
you to prompt you to check if you want to execute `COMMAND` on the said file
find . -ok -exec COMMAND {} \;

# match on files with said name
find . -name "*.py"
# match if the size is bigger than 2M
find . -size +2M
# match if size is less than 2M
find . -size -2M
# match by specific user but NOT from a specific  group or type (d for dir, f for file)
find . -type f -user root -not daemon
# match by creation time 
# we can use mtime, mmin (mins), we can use the minus and positive notations
# for more or less much more like in the -size notation

find . -mtime 0 # catch files that were modified today for example
# hell we can even search by permissions,  for example to look for setuid files
find . -type f -perm 4000 
```

It's also good to mention that `-type` also includes symlinks `l` and blocks
and character blocks `b/c`. Find has plenty of options and when in doubt refer
to the manual. 
```

####
<h3>ACL tools in RHEL </h3> 
RHEL uses fancy tools like `getfacl` and `setfacl` which enables extensive
control over files and who can access what in a specific way. In order to to
use it the system itself must support it by getting mounted with the
`user_xattr acl` option in order to work. You can read more about that at
https://wiki.archlinux.org/index.php/Access_Control_Lists

- `getfacl`: prints the current ACLs set on said file/directory by `setfacl`
- `setfacl`: sets permissions a file

the roles for setfacl are simply. It works the same as chmod as long as you
don't specify a group or a user.

```bash
# to get the ACLs of a certin file we simply just `getfacl`
getfacl /etc/passwd

# to set ACL for a specific user then just ( m = set )
setfacl -m u:tyr4n7:ACL_LEVEL_NUMBER filename # where ACL_LEVEL_NUMBER can be rwx or
7 or any thing less than that

# to set an ACL for a specific group then:
setfacl -m g:groupname:ACL_LEVEL_NUMBER (OCTAL or string format)
# to delete the ACl for a specific user 
setfacl -x u:tyrn47 filename

# to erase all the ACLs we simply just
setfacl -b filename

```

