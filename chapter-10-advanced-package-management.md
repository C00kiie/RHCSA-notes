<h1> Chapter 10: Advanced Package Management </h1>

This is the second of the two chapters that discusses softwareadministration in RHEL. 
While the first chapter (Chapter 09)expounds upon the basic concepts of rpm package 
management anddemonstrates. a basic command to manipulate individual packages, this 
chapter elaborates on the concepts and handling of package groups, packagemodules,
and package streams.

It also presents a coverage on the concept ofsoftware repositories and how to configure them.

As a start off it's good to know that RHEL in general separates packages in their repos

1- BaseOS: This includes the kernel, bootloader and other foundational software that are necessary for the system to function
2- AppStream: This includes pretty much everything else
3- Devel: Includes the necessary header files and development compilers and pretty much anything that is required to develop anything within RHEL environment

**Modules**: A nice way to put multiple streams (Like baseOS / AppStream / Devel) into one module, it can be enabled or disabled. If it's enabled you can query it and install stuff from it
For example perl provides two independent streams for the same software, and you can choose either of them

**Module Profile**: A module profile is a list of recommended packages oragnized for purpose-built, convenient deployments to support a variety of use cases such as 
minimal, development, common, client, server, etc. A profile may also include packages from the BaseOS repository or the dependencies of the stream. 
Each module stream can have zero, one, or more profiles associated with it with only one of them marked as the default

<h3> Repos </h3>
All the repos are located in /etc/yum.repos.d/ directory. The values you define in individual repoistories of the /etc/yum/

a sample repo looks like the following:

```bash
[BaseOS_RHEL_8.0]name= RHEL 8.0 base operating system components
baseurl=file:///mnt/BaseOS
enabled=1
gpgcheck=0
```




<h3> Managing Stuff with DNF</h3>


Dnf comes preconfigured in RHEL. You can view the configuration in /etc/dnf/dnf.conf
It looks a bit like this:

```bash
[main]
gpgcheck=1
installonly_limit=3
clean_requirements_on_remove=True
best=True
```

the format is the same as debian based distros
```bash
dnf <command> <target-package>
```
Here are the commands list: 
- check-update
- clean
- history
- info
- install
- list
- provides
- repoquery
- repolist
- search
- upgrade

It also has subcommands for groups packages

```
dnf group <command>
dnf module <command>
```
so the list is like:

- group install
- group info
- group list
- module disable
- module enable
- module reset
- module update
- module info
- module remove
- module install 

In any case,

- to review installed packages dnf repoquery
- to limit the query to one repo:  ` dnf repoquery --repo "BaseOS"` which will select packages only of that package
- enlist all packages from all repos that should be able to update `dnf list updates`
- to check only one package ` dnf check-update $package-name --repo "BaseOS"`
- to list only recent packages `dnf list recent`
- to install an rpm through dnf `dnf localinstall /path/to/rpm/file.rpm`
- to get all info of a package just simply do ` dnf info $package-name `
- to review installed packages `dnf list installed `
- to check which package provides which file you can do ` dnf provides /etc/passwd `
- you can use "whatprovides" which can accept wildcards

<h3> Group Packages Management </h3> 

- to list all groups:  ` dnf group list `, or you can get a summary:  `dnf group summary`
- to review the full list including the hidden ones: ` group list hidden`
- to install a group just go with `dnf groupinstall "$ GROUP NAME"`
- to remove a group you can go with `dnf groupremove "$ GROUP NAME"`



<h3> Module Management </h3>

Modules are just preferences, and you could literally just know two commands and be good with it

- `dnf modules list` which will show all enabled modules
- `dnf module install perl:$VERSION` which will install one of the modules streams

beyond that it's just literally all semantics and and stupid bs that nobody asked for. Unless you are looking for a hot quick deployment, modules are only a fancy thing.







NOTE: DO The exercises before attempting to do the exam



