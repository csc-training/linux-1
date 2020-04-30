---
title:  File permissions
author: CSC Training
date:     2020-04
lang:     en
---
# Contents of this session

UNIX/Linux (POSIX) file systems are structured into users and groups with corresponding permissions for access to directories and files. This session covers how to

- interpret file and directory attributes
- change file and directory attributes

# File attributes

- POSIX distinguishes between **users** (`u`), **groups** (`g`) and **others** (`o`)
- Each user belongs at least to one group
	- on distros, the user usually have their own group
	- check your (eventually multiple) groups:
	```bash
	$ groups
	```
- POSIX knows three main file attributes: 
	- `r` = readable
	- `w` = writeable
	- `x` = executable
	
# File attributes
- displaying file/directory attributes
  ```bash
  $ ls -l file
  $ ls -d -l directory
  ```
- resulting, e.g.,  in the output
  ```bash
  -rw-r--r-- 1 userid groupid 1024 Jan 29 11:04 file
  drwxr-xr-x 2 userid groupid 4096 Jan 29 11:04 directory
  ```
	- the first digit is the type of file: `-` for regular file, `d` for directory, `l` for link
	- next three digits indicate rights for user (here `rw` or `rwx`)
	- next three digits indicate rights for group (here `r` or `rx`)
	- next three digits indicate rights for others (same as group)
	- followed by: number of references, userid, groupid, modification-date
	
# File permissions	

- Lets copy a file to test a few things:
  ```bash
  $ cp /etc/group testfile
  $ ls -l testfile
  -> -rw-r--r-- 1 cscuser cscuser 1024 Apr 28 23:14 testfile
  ```
  - read + writeable for user, readable for group and everyone else
- Command to change permissions:
  ```bash
  $ chmod u+x,g+rw,o-r testfile
  $ ls -l testfile
  -> -rwxrw---- 1 cscuser cscuser 1024 Apr 28 23:14 testfile
  ```
  - `u` for users, `g` for groups, `o` for others; `-` deprives, `+` adds attribute
  - result: read, write and executable for user, read and writeable for group, no rights for others
	
# File permissions

- **Exercise**: do the following
  ```bash
  $ chmod u-r testfile
  ```
  - and then try
  ```bash
  $ less testfile
  ```
  - Copy the resulting output to eLearn and explain it
  - Derive a rule for contradicting attributes between user and group - which one determines the permissions?
	
# Changing file ownerships

- Changing file ownership to other user (might need privileged user rights to do so):
  ```bash
  $ chmod root testfile
  ```
- Changing file group (only possible for groups one is part of):
  ```bash
  $ chgrp lp testfile
  $ ls -l testfile
  -> -rwxrw---- 1 cscuser lp 1024 Apr 28 23:14 testfile
  ```
- Recursively changing attributes: both commands can act on whole directory trees, if argument is a directory and the option `-R` is added	

# Our first executable script

- Scripts are something similar like bash-macros, i.e., a series of commands that one can repeat by calling the script
	- Rule for happy computing: "As soon as you do a series of commands twice in the shell, it already pays off to make a script out of it"
- Using the text editor of your choice, open a new file called `befriendly.sh` with the following contents
  ```bash
  #!/bin/bash
  echo "Hello and welcome"
  echo "Today is:"
  date
  echo "Have a nice rest of the day"
  ```
	
# Our first executable script	
- **Exercise**: change the file for you as a user to be executable and then execute with
  ```bash
  $ ./befriendly.sh
  ```
  - NB: the `./` in front of your script is always needed for security reasons
  - Copy the output into Moodle
	
