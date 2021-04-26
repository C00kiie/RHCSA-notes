<h1> Chapter 3 </h1>

Objectives: 
- Common types of file in Linux
- Compress and uncompress files
- Archive and compress files and directories
- Edit files with the vim editor
- Create, list, display, copy, move, rename, and remove files and
- directories
- Create file and directory links
- Identify differences between copying and linking

This chapter is mainly about archiving and de-archiving. It also explains basic
things like naming conventions and symlinks. It also goes into things we
explained in the previous chapter about file types (Regular files, directories,
block and char devices) and now it introduces a new filetype called symlink 

###
<h3> Section 0: Symlinks </h3>

Symlinks are much more like shortcuts in windows. They only point to a file
that exists somewhere else

```bash
#     this_points_to -> this
ln -s SHORTCUT_POSITION SHORTCUT_REAL_FILE
 /etc/passwd /tmp/passwd
$ ls -la /tmp/passwd
lrwxrwxrwx 1 tyr4n7 tyr4n7 11 Mar  9 02:29 /tmp/passwd -> /etc/passwd
```
This can be used to create a soft link (a symlink here is also a file stored on
hard disk) and its contents are pointing on another inode in the hard disk. You
can create symlinks for files and directories with softlinks, however without
the s argument, we can get hardlinks which are only used for files which makes
another pointer to the same inode. The hardlinks cannot be used to symlink
directories

Note that the latter has to be an absolute link or elsewhat it will look for
the file relatively to the symlink shortcut location!



###

<h3> Section 1: Compressing / Decompressing Files </h3>

There are several utilities that archive files in linux: 
- gunzip

```bash
$ echo hi > test.txt
$ gzip test.txt
$ ls -la
total 0
drwxr-xr-x 1 tyr4n7 tyr4n7 512 Mar  9 02:08 .
drwxrwxrwt 1 root   root   512 Mar  9 02:08 ..
-rw-r--r-- 1 tyr4n7 tyr4n7  32 Mar  9 02:08 test.txt.gz
```

- tar
tar can make a tarball file (.tar extension). It can also be used in
conjuncation with other tools like gunzip (.tar.gz) natively. It has a pretty
cool set of arguments too.

Noteablly the `-p` option which preserves permissions.

  - To create a tarball file out of a file


```bash
$ rm *
$ echo hi > test.txt
$ tar cvf tarname.tar test.txt
test.txt
$ ls -la
total 12
drwxr-xr-x 1 tyr4n7 tyr4n7   512 Mar  9 02:16 .
drwxrwxrwt 1 root   root     512 Mar  9 02:08 ..
-rw-r--r-- 1 tyr4n7 tyr4n7 10240 Mar  9 02:16 tarname.tar
-rw-r--r-- 1 tyr4n7 tyr4n7     3 Mar  9 02:16 test.txt
```

  - To create a gunzip tarball: Same as before, just add an z to the arguments.
    Make sure to append a .tar.gz in the filename for clairty. You also have to
    add the z in extraction
  - To extract files:

```bash
$ tar cvf tarname.tar test.txt
test.txt
$ rm test.txt
$ tar xvf tarname.tar
test.txt
$ ls
tarname.tar  test.txt
```

We can also append to a tar file (hence the name) by adding an (R) or (U) in
the beginning. R adds stuff at the ending, U adds them at the beginning.

```bash
tar -rvf mytarfile.tar /extra/file/path
```

This can be useful to keep to keep appending to archives without constantly
deleting and creating new one which is a waste of time and space.

  - lastly but not the least, we can use the (J) option to bzip2 the tarball
    after tarball archive.

- bzip2/bunzip2
it has the exact same usage as gunzip.
```bash
$ echo hi > test.txt
$ bzip2
$ bzip2 test.txt
$ ls
test.txt.bz2
$
```



<h2> bzip2 vs gunzip </h2>
They do the same things but the only difference is that bzip2 has a better
compression ratio but is farily slower and consumes more CPU. (TIME VS SPACE)
[1]
###

<h3> Section 2: VIM usage  </h3>

Vim has a varity of shortcuts that are proven useful:

<h2> insertion </h2>
- i insert at the cursor position
- I insert at the beginning of the current line
- a append after the cursor position
- A append text to the end of the current line
- o insert a newline below the current line
- O insert a newline atop the current line

<h2> Navigation </h2>

- k/j upward/downward
- h / l forward/backward

<h2> deletion </h2>

- x delete character at the cursor position
- X delete the character before the cursor position (much more like DEL)
- dw deletes the word or part of it to the right of the cursor
- dd deletes current line
- D deletes from current position to the end of the line to the right
- :6,12d deletes from 6 to 12 in terms of lines

<h2> undoing </h2>

- u undo last change
- U undo last change on current line
- :u undoes the previous line mode command
- . repeats last command

<h2> Searching </h2>

- /string search forward 
- ?string search backward
- n find next occurence
- N find previous occurence

<h2> replacing </h2>

as simple as `:%s/old/new` for one instance, and `%s/olg/new/g` for all
instances in the current opened file



####

<h3> Create Directories and files </h3>

Look this up and try it yourself. I'll add only highlights/important things

- We can cerate a new file with a custom date like this

```bash
touch -d 2019-09-20 filename
```
- to change a file creation date to current date: 
```bash
touch -m filename
```


<h3> Misc . </h3>

- we can use tac (cat backwards) to cat files backwards from top to bottom
- use less, it uses the same concepts as vim
- head and tail both accept -n which tells it how many lines it should read
- use `tail -f` to keep reading off a file (pretty useful)
- `cp -r directory new_directory` copies an entire directory
