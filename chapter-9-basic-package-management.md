<h1> Chapter 9: Basic Package Management YUM</h1>

Objectives:
- Package management in Centos/Redhat 
- Packages dependency and database
- Query, install, freshen, overwrite and remove packages
- Validate package integrity
- View GPG Keys and verify package attributes
- Manage packages using the rpm command


<h3> RPMS </h3>
RPMs are just nicely tied up tar files. When you do sudo rpm -i $package.rpm,
it's only unpacking stuff to the right place and that's it. It also has more
information about the package and what it includes so it can keep track of what
files are owned by what packages. Much more like tar files, each compressed
part inside it has directory (position) and the permissions associated with it.

All metadata related to packages is stored at a central location and includes
information such as package version, installation location, checksum values,
and a list of included files with their attributes. This allows the package
management toolset to handle package administration tasks efficiently by
referencing this metadata.

*side-notes*:
- normally an rpm package consists of 5 parts:
  $packageName-$Version-$PackageReleaseOrRevision-$distro-ProcessorArchitecture
  here's an example: openssl-1.1.1-8.el8.x86_64.rpm
- meta data is stored in `/var/lib/rpm` directory. It's called the packages
  database

<h3> RPMs tools: Yum </h3>
The primary tool in RHEL 8 is yum, however it's only a soft link to another
tool called `dnf` 

The rpm command handles package management tasks including querying,
installing, upgrading, freshening, overwriting, removing, extracting,
validating, and verifying packages. As mentioned, this command has a major
drawback as it does not have the ability to automatically satisfy package
dependencies, which can be frustrating during software installation and
upgrade. The rpm command works with both installed and installable
packages

Here's a list of useful commands with yum

- `rpm -qa` lists all packages installed in a system
- `rpm -qf $filepath` lists which package owns the side $filepath
- `rpm -ql $packageName` lists all files that a package would install
- `rpm -qd $packagename` lists only the documentation files inside an RPM
  package
- `rpm -ivh $package.rpm` where i=install, v=verbose, h=hash should give you
  a good vision on the installation of said $package
- `rpm -Uvh $package.rpm` is the same as installation above, except U goes for
  updating an already residing package. It'd go thorugh all files and replace
  them if they exist. It also keeps a backup of all configuration files
  belonging to the said package and labels it as $configuration_file.rpmsave,
  instead of removing it. If the package isn't installed it'll go ahead and
  install it
- `rpm -Fvh` is the same as updating except it won't install it if it weren't
   already installed
- `rpm -ivh -replacepkgs $package.rpm` will overwrite the package files with no
  backups at all. It's not recommended
- `rpm -ev $packageName` removes an already installed package and erases it,
  too, it will fail if the package is being using by another package (A
 dependency)
-  `rpm2cpio $package.rpm` can be used to inspect package's files before
   installation. Assume you deleted something like /etc/chrony.conf. You can
   retrieve the file by extracting the package files and then moving the file
  the same place.


<h3> Validating Package integrity and Crediblity </h3>
RHEL 8 includes a very easy way to verify the integrity and credibiltiy of
a package by simply doing
- `rpm -K $package.rpm`
which returns
`$package.rpm: digests OK`
which confirms it's corrupt-free

- we can inspect our public keys by `rpm -qil gpg-pubkey`
  which will list all public keys and from where they were imported, too

- We can check a package's file attributes by comparing the package files
  attributes to the installed files on the system by `rpm -V $package-name`
  if everything is alright, it'll simply not return anything. If any attribute
  changed at any given time, it'll notify you and let you know in the command
  command line. You can also verify one file by `rpm -Vf $filename`. Below
  a list of possible changes and what they mean

- S -> Size
- M -> Permissions
- 5 -> Checksum doesn't match
- D -> device and its minor or major number changed
- L -> appears if a symlink path has been changed
- U -> Ownership
- P -> Capabilities
- G -> Group
- T -> Timestamp

The file type is determined by the following list

- c conf gile
- d documentation file
- g ghost file?
- l license file
- r readme file


P.S: RHEL 8 stores gpg-pubkeys in /etc/pki/rpm-gpg

and that's all you need to manage packages in RHEL 8!

