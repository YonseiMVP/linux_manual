Link
====

In some cases, we need a file (or a directory) has multiple names.

**symbolic link**, or **link** in short form, is a special file that points
other file or directory.

Create and Remove link
----------------------

### Create link

`ln -s <target file> <new link>`

If `<new link>` is omitted, target's filename is used.

```console
$ ln -s original link
$ ls -al
total 12
drwxrwxr-x  2 koasing koasing 4096 12월  7 11:17 .
drwxr-xr-x 23 koasing koasing 4096 12월  7 11:17 ..
lrwxrwxrwx  1 koasing koasing    8 12월  7 11:17 link -> original
-rw-rw-r--  1 koasing koasing   29 12월  7 11:17 original
```

The link is special file, and marked as `l` in property section.

`<target file>` could be relative or absolute path.



### Remove link

Use `rm` to remove link. It removes link, but does not remove link target.

```console
$ rm link
$ ls -al
total 12
drwxrwxr-x  2 koasing koasing 4096 12월  7 11:35 .
drwxr-xr-x 23 koasing koasing 4096 12월  7 11:17 ..
-rw-rw----  1 koasing koasing   29 12월  7 11:17 original
```



Accessing link
--------------

Accessing to link is automatically de-referenced, or redirected to  its target.
In other words, link works as like its target.

```console
$ cat original
This is link original file.

$ cat link
This is link original file.

$ chmod 600 link
$ ls -al
total 12
drwxrwxr-x  2 koasing koasing 4096 12월  7 11:17 .
drwxr-xr-x 23 koasing koasing 4096 12월  7 11:17 ..
lrwxrwxrwx  1 koasing koasing    8 12월  7 11:17 link -> original
-rw-------  1 koasing koasing   29 12월  7 11:17 original
$ cp link new_file
$ cat new_file
This is link original file.

$ ls -al
total 16
drwxrwxr-x  2 koasing koasing 4096 12월  7 11:30 .
drwxr-xr-x 23 koasing koasing 4096 12월  7 11:17 ..
lrwxrwxrwx  1 koasing koasing    8 12월  7 11:17 link -> original
-rw-------  1 koasing koasing   29 12월  7 11:30 new_file
-rw-------  1 koasing koasing   29 12월  7 11:17 original
```



### Permission of link

Usually, link itself has 777 permission. During dereferencing, system checks
target's permission.

```console
$ ls -al
total 12
drwxrwxr-x  2 koasing koasing 4096 12월  7 11:37 .
drwxr-xr-x 23 koasing koasing 4096 12월  7 11:17 ..
lrwxrwxrwx  1 koasing koasing    8 12월  7 11:37 link -> original
----------  1 koasing koasing   29 12월  7 11:17 original
$ cat link
cat: link: Permission denied
```

In some UNIX-like system like FreeBSD, permission of link itself could be
changed. Linux does not permit that operation.



### Broken link

Even link target is removed, link itself still exists, because link is another
file. In this case, accessing to broken link is considered as accessing to
non-existing file.

```console
$ ls -al
total 8
drwxrwxr-x  2 koasing koasing 4096 12월  7 11:52 .
drwxr-xr-x 23 koasing koasing 4096 12월  7 11:17 ..
lrwxrwxrwx  1 koasing koasing    8 12월  7 11:52 link -> original
$ cat link
cat: link: No such file or directory
$ echo "This is new text" > link
$ ls -al
total 12
drwxrwxr-x  2 koasing koasing 4096 12월  7 11:55 .
drwxr-xr-x 23 koasing koasing 4096 12월  7 11:17 ..
lrwxrwxrwx  1 koasing koasing    8 12월  7 11:52 link -> original
-rw-rw-r--  1 koasing koasing   17 12월  7 11:55 original
$ cat original
This is new text
```



Directory link
--------------

Link could point to directory.

```console
$ ls -al
total 12
drwxrwxr-x  3 koasing koasing 4096 12월  7 11:59 .
drwxr-xr-x 23 koasing koasing 4096 12월  7 11:17 ..
drwxrwxr-x  2 koasing koasing 4096 12월  7 12:00 test01
$ ls -al test01
total 12
drwxrwxr-x 2 koasing koasing 4096 12월  7 12:00 .
drwxrwxr-x 3 koasing koasing 4096 12월  7 11:59 ..
-rw-rw-r-- 1 koasing koasing   23 12월  7 12:00 original
$ cat test01/original
This is original text.
$ ln -s test01 link
$ ls -al
total 12
drwxrwxr-x  3 koasing koasing 4096 12월  7 12:00 .
drwxr-xr-x 23 koasing koasing 4096 12월  7 11:17 ..
lrwxrwxrwx  1 koasing koasing    6 12월  7 12:00 link -> test01
drwxrwxr-x  2 koasing koasing 4096 12월  7 12:00 test01
$ cd link
$ ls -al
total 12
drwxrwxr-x 2 koasing koasing 4096 12월  7 12:00 .
drwxrwxr-x 3 koasing koasing 4096 12월  7 12:00 ..
-rw-rw-r-- 1 koasing koasing   23 12월  7 12:00 original
$ cat original
This is original text.
$ touch newfile
$ cd ../test01/
$ ls -al
total 12
drwxrwxr-x 2 koasing koasing 4096 12월  7 12:02 .
drwxrwxr-x 3 koasing koasing 4096 12월  7 12:00 ..
-rw-rw-r-- 1 koasing koasing    0 12월  7 12:02 newfile
-rw-rw-r-- 1 koasing koasing   23 12월  7 12:00 original
```



### Evaluating `.` and `..` in directory link

bash builtin command - `.` and `..` consider link itself.  
system call - `.` and `..` consider target, not link path.

```console
$ ls -al
total 8
drwxrwxr-x  2 koasing koasing 4096 12월  7 13:55 .
drwxr-xr-x 24 koasing koasing 4096 12월  7 13:55 ..
lrwxrwxrwx  1 koasing koasing   18 12월  7 13:55 test -> ~/test
$ cd test
$ ls ..
anaconda3  Desktop    Downloads         Music     Public     test   Videos
data       Documents  examples.desktop  Pictures  Templates  test2
$ cd ..
$ ls
test
```



### Removing directory link

Because link itself is a file, use `rm` to remove link, even it points to
directory.

```console
$ ls -al
total 12
drwxrwxr-x  3 koasing koasing 4096 12월  7 12:00 .
drwxr-xr-x 23 koasing koasing 4096 12월  7 11:17 ..
lrwxrwxrwx  1 koasing koasing    6 12월  7 12:00 link -> test01
drwxrwxr-x  2 koasing koasing 4096 12월  7 12:02 test01
$ rmdir link
rmdir: failed to remove 'link': Not a directory
$ rm link
$ ls -al
total 12
drwxrwxr-x  3 koasing koasing 4096 12월  7 12:05 .
drwxr-xr-x 23 koasing koasing 4096 12월  7 11:17 ..
drwxrwxr-x  2 koasing koasing 4096 12월  7 12:02 test01
```



### Recursive directory link

Be careful not creating an recursive directory structure (link pointing to its
direct ancestor directory). Most tools handle recursive link properly, but
some tools could fail into infinite loop.



Why use link?
-------------

### CASE : Python

There are multiple python instances reside in single system. These installation
could be easily referrenced using symbolic link.

```console
$ ls -al python*
lrwxrwxrwx 1 root root       9 11월 17 15:16 python -> python2.7
lrwxrwxrwx 1 root root       9 11월 17 15:16 python2 -> python2.7
-rwxr-xr-x 1 root root 3542008 11월 24 02:08 python2.7
lrwxrwxrwx 1 root root       9 11월 17 15:16 python3 -> python3.5
-rwxr-xr-x 2 root root 4464400 11월 29 01:53 python3.5
-rwxr-xr-x 2 root root 4464400 11월 29 01:53 python3.5m
lrwxrwxrwx 1 root root      10 11월 17 15:16 python3m -> python3.5m
$ python --version
Python 2.7.12
$ python2 --version
Python 2.7.12
$ python3 --version
Python 3.5.2
$ python3.5 --version
Python 3.5.2
```



Hardlink
--------

There is other type of link, **hardlink**.

Symbolic link is a file that points to other file.
Hardlink is a duplicated file entry point (inode).

```console
$ ln original hardlink
$ ln -s original symlink
$ ls -al --inode
total 16
557869 drwxrwxr-x  2 koasing koasing 4096 12월  7 12:15 .
540807 drwxr-xr-x 23 koasing koasing 4096 12월  7 12:11 ..
557907 -rw-rw-r--  2 koasing koasing   23 12월  7 12:00 hardlink
557907 -rw-rw-r--  2 koasing koasing   23 12월  7 12:00 original
557831 lrwxrwxrwx  1 koasing koasing    8 12월  7 12:14 symlink -> original
```

The two files, `original` and `hardlink` have same inode number, while `symlink`
has different inode number.

After deleting original file, the difference is obvious.

```console
$ rm original
$ ls -al
total 12
drwxrwxr-x  2 koasing koasing 4096 12월  7 13:32 .
drwxr-xr-x 23 koasing koasing 4096 12월  7 12:11 ..
-rw-rw-r--  1 koasing koasing   23 12월  7 12:00 hardlink
lrwxrwxrwx  1 koasing koasing    8 12월  7 12:14 symlink -> original
$ cat hardlink
This is original text.
$ cat symlink
cat: symlink: No such file or directory
```
