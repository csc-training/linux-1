---
title:  A second look at the shell
author: CSC Training
date:     2020-04
lang:     en
---
# Contents of this session

In this session, we dive a little deeper into shell commands and then discuss a series of commands you might be in need of during your work on your own Linux desktop, but also on remote machines. We cover issues like

- finding files
- searching contents inside files
- managing disk space
- secure connection
- archiving of files

# So, again, what is shell?

- Text only user interface
	- Optimized to work with files --- and everything is a file (text) in Linux
- Programmable User Interface
  - Bash is a Turing-complete programming language
  - (With some practice) It is easy to write new small programs/commands, scripts
  - That is why it is used in professional computing: **Be a programmer, not a computer!**
  
# The moment you press ENTER

- Bash will ...
  - expand variables and file name wildcards (globbing)
  - split the line into “words”
  - connect files and pipes
  - interpret the first word as the name of the command, and the rest of the words as the arguments to the command

# Commands

- Let's break down the following command:

```bash
$ echo "Hello!" > foo.txt
```
  
stdin ->   | `echo`        | `"Hello!"` | `> foo.txt`
-----------|---------------|------------|------------
input from other program or keyboard; not used here  | first word = command | argument to command | **stdout** into `foo.txt`, **stderr** to terminal, **exit code** (0=success, else fail)

# White spaces in file names

- Lets assume we (want to) have a file with a space (like often in Windows)
- We want to fill the file `Operating System.txt` with the words `Linux`
  ```bash
  $ echo "Linux" > Operating System.txt
  ```
  - What happens when you make a listing of the directory?
- Instead of adding spaces into your filename, you could use *snake_case* or *CamelCase* style, i.e., `Operating_system.txt` or `OperatingSystem.txt`
- If you insist, use either single quotes or escape `\` character:
  ```bash
  $ echo "Linux" > 'Operating System.txt'
  $ echo "UNIX" >> Operating\ System.txt
  ``` 
  
# Finding files

- The hard way: `cd` yourself trough the file-tree followed by `ls`
- The easy way:
  ```bash
  $ find /etc -name "*.conf" -print
  ```
  - searches directory (here `/etc`) recursively and prints matching filenames (here all files with suffix `.conf`)
  - this is a real-time search on the disk, hence produces heavy I/O 
- The correct way:
  ```bash
  $ locate *.conf
  ```
  - accesses a database and prints matching files
  - database is not real-time (can miss new files)
  
# Finding contents

- Search for the occurrence of the word *network* in any file under `/etc/init.d`
  ```bash
  $ grep -i network /etc/init.d/*
  ```
  - `-i` makes search case-insensitive (i.e., it will find `network`, `Network` and `NetWork`)
- Recursively search whole directory-tree (here `/etc`)
  ```bash
  $ grep -r network /etc
  ```
  - getting rid of error messages (redirecting **stderr** by `2>`)
    ```bash
  $ grep -r network /etc 2>/dev/null
  ```
  - everything you send into the device `/dev/null` will disappear 
  
# Pipes
  
- **Piping** with `|` for whole workflow of commands:
  ```bash
  $ grep -r network /etc 2>/dev/null | grep start | less
  ```
  - finds any line with `network` under `/etc`-tree (ignoring stderr) and then selects those with the word `start` in the same line and pages (`less`) result on screen
- Pipes direct the **stdout** of the previous into the **stdin** of the following command
- **Exercise**: try to find out whether **stderr** is passed on by a pipe

# Managing space

- Rule of life: Computer disk-space always is too small (no matter how much you buy)
- How much space is there left on my devices?
  ```bash
  $ df -h
  ```
  - option `-h` for *human readable* output
  ```
  Filesystem      Size  Used Avail Use% Mounted on
  udev            461M     0  462M   0% /dev
  tmpfs            99M  1,1M   98M   2% /run
  /dev/sda5        11G  7.4G  2.3G  73% /
  tmpfs           493M     0  493M   0% /dev/shm
  tmpfs           5.0M  4.0K  5.0M   1% /run/lock
  tmpfs           493M     0  493M   0% /sys/fs/cgroup
  tmpfs           796M   64K  796M   1% /run/user/1000
  ```


# Managing space

- Identifying disk-space consumption of subdirectories (here in `/usr`):
  ```
  $ du -sh /usr/*
  ```
- options `-h` for *human readable* output and `-s` for sum (else it would step into each subdirectory)
  ```
  392M    /usr/bin
  44K     /usr/games
  227M    /usr/include
  3.8G    /usr/lib
  832K    /usr/libexec
  172K    /usr/local
  45M     /usr/sbin
  2.7G    /usr/share
  940M    /usr/src
  ```


# Login to a remote machine from the local terminal

- Secure Shell (SSH):
  ```bash
  $ ssh –X name@target.computer.fi
  ```
  - option `-X` (or `–Y`) allows *tunneling* application windows so that they open on your local machines screen. 
- **Exercise**: With the information given on the course-page, log into the target computer using `ssh`-command. To proof that you were there, create an empty file (there was a command for this!) in the home-directory that contains your given and your family-name separated by a space, so something like: `Elvis Presley.txt`  (remember, to correctly use the escape character to get the space)	
	
# Downloading files from remote URLs

- You can use a browser, but ...
- `wget` allows to download URL's in the shell 
  ```bash
  $ wget http://ftp.gnu.org/gnu/hello/hello-2.7.tar.gz
  ```
  - mind that some URL's can produce weird download names (option `-O` might help)
  - **Exercise**: verify with `ls`, whether the creation date of the downloaded file is preserved or not.
  
# Copying files to and from remote machines

- `scp` (secure-copy) copies your files encrypted between different machines
- copying to remote machine  
  ```bash
  $ scp  hello-2.7.tar.gz user@puhti.csc.fi:'~/'
  ```
  - **Exercise**: Explain, where on `puhti` will you find the file?
- copying from remote machine
  ```bash
  $ scp user@puhti.csc.fi:'~/hello*.tar.gz' . 
  ```
  
# Compressing files and directories 

- `tar` is one of the oldest commands and stands for *tape archive*. It takes whole directories and puts their files sequentially (like on a tape) in a single file.
  - because of its legacy, the options (`t`=text/contents, `c`=create, `x`=extract, `v`=verbose, `z`=compress with `gzip`, `f`=file) can be given without dash `-`
  - **list** (don't extract) contents
  ```bash
  $ tar tvzf hello-2.7.tar.gz
  ```
  - **unpack** the file with
  ```bash
  $ tar xvzf hello-2.7.tar.gz
  ```
  - **create** a new (now uncompressed) `tar` archive
  ```bash
  $ tar cvf myownhello-2.7.tar hello-2.7/* 
  ```
  
# Compressing files and directories 

- **Exercise**: Make a listing showing the size of `hello-2.7.tar.gz` and compare it to the summed size of the newly unpacked subdirectory `hello-2.7` (we learned this commands a slides ago). Also compare it to the size of the `myownhello-2.7.tar`. Check the following slide to explain the difference in sizes

# Compressing files and directories 

  - In fact, the file `hello-2.7.tar.gz` is a gnu-zipped tar archive. The compression feature is built into `tar` and can be issued with `z` option
- `gzip` is the GNU version of `zip` (you might know from Windows).
- To obtain a pure uncompressed `tar` archive:
  ```bash
  $ gzip -d hello-2.7.tar.gz
  ```
  - this decompresses and removes the suffix `.gz`
  - you could use `gunzip` instead of `gzip -d`, where the option stands for *decompress*
- to create a new `gzip` file (`-9` means highest level of compression)
  ```bash
  $ gzip -9  myownhello-2.7.tar
  ```
  
# Compressing files and directories  
  
- *My colleague sent me a Windows ZIP file, what should I do?*
- There is a ZIP-compatible command on Linux/UNIX
- To **create** `hello.ZIP` recursively (`-r`) from the subdirectory `hello-2.7`
  ```bash
  $ zip -r hello.ZIP hello-2.7/
  ```
- To **extract**
  ```bash
  $ unzip hello.ZIP
  ```
- To only **list** contents
  ```bash
  $ unzip -l hello.ZIP
  ```
  
# This is an advertisement!

- We barely scratched the surface of possible commands in Linux/UNIX.
- With the presented set you can *survive* 
- But perhaps you want to go beyond this level?
  - There are commands you can do in the shell that basically replace spread-sheet functionality, like pasting, cutting columns and rows, etc. (`head`, `tail`, `cut`, `paste`)
  - Even programmable text-editors (`sed` and `awk`) exist
  - If you work on scientific (usually tabular) data, you have an operating system that delivers at its user interface (no additional programs needed)
  
  
  
