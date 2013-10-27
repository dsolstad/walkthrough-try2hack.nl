#### Level 13

I used the Linux distro Kali to solve this challenge, but any distro will do. If your distro doesn't have the tools used in this guide, just install the sleuthkit package. 
On Ubuntu/Debian systems: `sudo apt-get install sleuthkit`

First we download the file:
`root@kali:~# wget http://try2hack.nl/levels/level13`

Then we can find out what kind of file this is:
```bash
root@kali:~# file level13 
level13: Linux rev 1.0 ext3 filesystem data, UUID=568b6779-f529-4164-8526-c1053a745272 (needs journal recovery)
```

This means that the file is a ext3 filesystem. We may start by mounting it.

```bash
root@kali:~# mkdir /media/level13
root@kali:~# mount level13 /media/level13
```

Let's take a look at what's inside.

```bash
root@kali:~# ls /media/level13/
bin  etc  home  lost+found  sbin
```

The home folder looks interesting...

```bash
root@kali:~# ls /media/level13/home/
lamer
root@kali:~# ls /media/level13/home/lamer/
tlk-0.8-3.pdf
```

Just a pdf about the Linux Kernel. Maybe there is some hidden files here?

```bash
root@kali:~# ls -a /media/level13/home/lamer/
.  ..  .password_part1.txt  .password_part3.gif  tlk-0.8-3.pdf
root@kali:~#
``` 

Bingo. Looks like the password consists of three parts. 

```bash
root@kali:~# cat /media/level13/home/lamer/.password_part1.txt 
f1ff95085
root@kali:~#
```

There we have the first part. Now what's in .password.part3.gif?

We pipe the result to the tool "strings" to filter out the noise.

```bash
root@kali:~# cat /media/level13/home/lamer/.password_part3.gif | strings
GIF89ax
{{{uuukkkfffe]TUf7YYYRRRIIIBBBEA=G<2;;;;85
=3(333'%$
H[C\F_
+b4Gp
#P]{
d_\n
HTwN
>\5e}
root@kali:~#
```

Nothing interesting in the source. Viewing it in a image viewer just shows the try2hack logo. Let's go deeper into the filesystem...

The ext filesystem has inodes that holds metadata of files. We can browse the filesystem by going to inodes and get information om which data blocks the file or directory is located at. It will make much more sense when we start.

First we find the block size.

```bash
root@kali:~# fsstat -f ext level13 
FILE SYSTEM INFORMATION
--------------------------------------------
File System Type: Ext3
Volume Name: 
Volume ID: 7252743a05c12685644129f579678b56

Last Written at: Sun Oct 27 16:49:44 2013
Last Checked at: Sat Jul 13 14:34:41 2013

Last Mounted at: Sun Oct 27 16:49:44 2013
Unmounted properly
Last mounted on: 

Source OS: Linux
Dynamic Structure
Compat Features: Journal, Ext Attributes, Resize Inode, Dir Index
InCompat Features: Filetype, Needs Recovery, 
Read Only Compat Features: Sparse Super, 

Journal ID: 00
Journal Inode: 8

METADATA INFORMATION
--------------------------------------------
Inode Range: 1 - 17713
Root Directory: 2
Free Inodes: 14404

CONTENT INFORMATION
--------------------------------------------
Block Range: 0 - 70583
Block Size: 1024
Reserved Blocks Before Block Groups: 1
Free Blocks: 35311

BLOCK GROUP INFORMATION
--------------------------------------------
Number of Block Groups: 9
Inodes per group: 1968
Blocks per group: 8192
...
root@kali:~#
```

As you can see in the output, the block size is 1024, which we will remember for later.

Since we know the folders in the filesystem, we could just find the inode right away, instead of digging from inode number 2 (the root inode).

```bash
root@kali:~# ifind -f ext -n /home/lamer/ level13
11810
root@kali:~#
```

Now we know that the /home/lamer directory is refered to by the inode 11810.

We can use the tool "istat" to find the data blocks for that inode:

```bash
root@kali:~# istat -f ext level13 11810
inode: 11810
Allocated
Group: 6
Generation Id: 1644872091
uid / gid: 0 / 0
mode: drwxr-xr-x
size: 1024
num of links: 2

Inode Times:
Accessed:	Sun Oct 27 16:52:32 2013
File Modified:	Sat Jul 13 14:43:52 2013
Inode Modified:	Sat Jul 13 14:43:52 2013

Direct Blocks:
51713 
root@kali:~#
```

With the tool "dd", we can see the contents of the data block.

Basically dd is a tool to copy and convert stuff. <if> is the input file, <bs> is block size (remember we learned the block size earlier?), <count> is how many blocks we copy, <skip> is at which block we start to copy from. There is also <of> for output file and <seek> which is almost like skip, but it is the block for the output file we start to write to.

```bash
root@kali:~# dd if=level13 bs=1k count=1 skip=51713 | strings
1+0 records in
1+0 records out
tlk-0.8-3.pdf
.password_part3.gif
.password_part1.txt
.password_part2.txt
1024 bytes (1.0 kB) copied, 3.0092e-05 s, 34.0 MB/s
root@kali:~#
```

Well, look at that. .password_part2.txt is also showing now, but let's first focus on the part3 gif file.

We find the inode.

```bash
root@kali:~# ifind -f ext -n /home/lamer/.password_part3.gif level13 
11812
root@kali:~#
```

Then we find the data blocks.

```bash
root@kali:~# istat -f ext level13 11812
inode: 11812
Allocated
Group: 6
Generation Id: 1644872093
uid / gid: 0 / 0
mode: rrwxr-xr-x
size: 738
num of links: 1

Inode Times:
Accessed:	Sun Oct 27 16:57:57 2013
File Modified:	Sat Jul 13 14:42:43 2013
Inode Modified:	Sat Jul 13 14:42:43 2013

Direct Blocks:
56321
root@kali:~#
```

Then we view the data block.

 ```bash
 root@kali:~# dd if=level13 bs=1k count=1 skip=56321 | strings
1+0 records in
1+0 records out
1024 bytes (1.0 kB) copiedGIF89ax
{{{uuukkkfffe]TUf7YYYRRRIIIBBBEA=G<2;;;;85
=3(333'%$
H[C\F_
+b4Gp
, 1.9323e-05 s, 53.0 MB/s
#P]{
d_\n
HTwN
>\5e}
a82d839f0bd1b
root@kali:~#
```

The last line didn't show before. Looks like we found the third part of the password.

Now let's to the same thing with .password_part2.txt.

Again, we start by finding the inode.

```bash
root@kali:~# ifind -f ext -n /home/lamer/.password_part2.txt level13 
11997
root@kali:~#
```

Then the data blocks.

```bash
root@kali:~# istat -f ext level13 11997
inode: 11997
Not Allocated
Group: 6
Generation Id: 1644872278
uid / gid: 0 / 0
mode: rrw-r--r--
size: 0
num of links: 0

Inode Times:
Accessed:	Sat Jul 13 14:43:31 2013
File Modified:	Sat Jul 13 14:43:52 2013
Inode Modified:	Sat Jul 13 14:43:52 2013
Deleted:	Sat Jul 13 14:43:52 2013

Direct Blocks:
root@kali:~#
```

As you may notice, there isn't any direct blocks for this file. This is probably because it's deleted.
Let's confirm this with a tool that shows all deleted files.

```bash
root@kali:~# fls -rpd level13
...
r/r * 11997:	home/lamer/.password_part2.txt
...
root@kali:~#
``` 

There we have the file we are after. "ext3grep" is a good tool to retrieve deleted files. Download and install it with: `root@kali:~# apt-get install ext3grep`

Then we run the command: `root@kali:~# ext3grep level13 --restore-all`

The folder with all the restored files now got created.

```bash
root@kali:~# cat RESTORED_FILES/home/lamer/.password_part2.txt 
0ac2f1f51f
root@kali:~#
```

There we have the second part of the password.

Together all the parts form: `f1ff950850ac2f1f51fa82d839f0bd1b`, which looks like a md5 hash, but it is indeed the correct password.