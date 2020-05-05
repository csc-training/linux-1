---
title:  Setup of the system
author: CSC Training
date:     2020-04
lang:     en
---

# Contents of this session

UNIX/Linux system are configurable via certain variables. It will not catch you by surprise that this is mainly done using text-files. This session shall give you some insight on how to change the behaviour of your system. We will discuss:

- Difference between shell/environment variables
- declaration of variables and accessing their values
- unsetting variables
- automatic setup of your system.

# The environment

- When interacting with a host through a shell session, there are
many pieces of information that your shell compiles to determine
its behaviour and access to resources.
- The way that the shell keeps track of all of these settings and
details is through an area it maintains, called the environment.
- Every time a shell session spawns, a process takes place to gather
and compile information that should be available to the shell
process and its child processes.

# How the environment works

- The environment is implemented as strings that represent key-
value pairs and they generally will look something like this: `KEY=value1:value2:...`
	- By convention, these variables are usually defined using all capital letters.
- They can be one of two types, environmental variables or shell
variables.
	- **Environmental variables** are variables that are defined for the current shell and are inherited by any child shells or processes.
	- **Shell variables** are contained exclusively within the shell in which they were set or defined.

# Printing shell and environmental variables

- We can see a list of all of our environmental variables by using the
`printenv` command.
- The `set` command can be used for listing the shell variables.
	- If we type `set` without any additional parameters, we will get a list of all shell variables, environmental variables, local variables, and shell functions.
	- The amount of information provided by `set` is overwhelming and there is no easy way limiting the output to shell variables only.
	- You can still try with
	```bash
	$ comm -23 <(set -o posix; set | sort) <(printenv | sort)
	```

# Common environmental and shell variables

- Some environmental and shell variables are very useful and are referenced fairly often. Here are some **common environmental variables** that you will come across:
	- **`SHELL`**: This describes the shell that will be interpreting any commands you
type in. In most cases, this will be bash by default.
	- **`USER`**: The current logged in user.
	- **`PWD`**: The current working directory.
	- **`OLDPWD`**: The previous working directory. This is kept by the shell in order to switch back to your previous directory by running `cd -`.
	- **`PATH`**: A list of directories that the system will check when looking for
commands. When a user types in a command, the system will check
directories in this order for the executable.

# Common environmental and shell variables

- to continue from previous slide:
	- **`LS_COLORS`**: This defines colour codes that are used to optionally add
coloured output to the ls command. This is used to distinguish different file
types and provide more info to the user at a glance.

	- **`LANG`**: The current language and localization settings, including character encoding.
	- **`HOME`**: The current user's home directory.
	- **`_`**: The most recent previously executed command (actually last word)
	
# Common environmental and shell variables
-  to continue further:
	- **`COLUMNS`**: The number of columns that are being used to draw output on
the screen.
	- **`DIRSTACK`**: The stack of directories that are available with the `pushd` and `popd` commands.
	- **`HOSTNAME`**: The hostname of the computer at this time.
	- **`PS1`**: The primary command prompt definition. This is used to define what
your prompt looks like when you start a shell session.
	- **`PS2`** is used to declare secondary prompts for when a command spans
multiple lines.
	- **`UID`**: The UID of the current user.

# Setting shell and environmental variables

- Defining a shell variable is easy to accomplish; we only need to specify a name and a value:
  ```bash
  $ TEST_VAR='Hello World!'
  ```
  - We now have declared a shell variable, which is available in our current session, only. It will not be passed down to child processes. We can see this by grepping for our new variable within the `set` output:
  ```bash
  $ set | grep TEST_VAR
  ```
  - We can verify that this is not an environmental variable by trying the same thing with `printenv`:
  ```bash
  $ printenv | grep TEST_VAR
  ```
	
# Setting shell and environmental variables

-  In order to change a **shell variable into an environmental variable**, we have to `export` it:
   ```bash
   $ export TEST_VAR
   ```
   - We can check this by checking our environmental listing again:
   ```bash
   $ printenv | grep TEST_VAR
   ```
- Declaration of environmental variables in a single line:
  ```bash
  $ export NEW_VAR="Testing export"
  ```
- Accessing the value of any variable with  preceding **`$`**:
  ```bash
  $ echo $TEST_VAR
  $ ls $HOME
  ```
	
# Demoting and un-setting variables

- Demoting an environmental variable back into a shell variable using:
  ```bash
  $ export -n TEST_VAR
  ```
  - **Exercise**: From the previous slides, find the right command for testing environmental variable followed by `| grep TEST_VAR` (this syntax will be explained later) - what do you expect to see on the screen?
  - **Exercise**: Do the same as above with the command that lists all shell variables - is `TEST_VAR` still a shell variable?
- To completely unset a variable (either shell or environmental):
  ```bash
  $ unset TEST_VAR
  ```
  
# Setting variables at login

- We do not want to have to set important variables up every time
we start a new shell session, so how do we make and define
variables automatically?
- Shell initialization files are the way to persist common shell
configuration, such as:
	- `$PATH` and other environment variables
	- shell tab-completion
	- aliases, functions
	- key bindings

# Shell modes
 
- The bash shell reads different configuration files depending on
how the session is started. There are **four modes**:
	1. A **login shell** is a shell session that begins by authenticating the user. 
	2. If you start a new shell session from within your authenticated session, a **non-login shell** session is started.
	3. An **interactive shell** session is a shell session that is attached to a terminal (like the one you use).
	4. A **non-interactive shell** session is one that is not attached to a terminal session.	
- Each shell session is classified as either login/non-login and interactive/non-interactive (so 4 possible permutations)


# Some common operations and shell modes

|Operation                      |Shell modes		           |
| ----------------------------- |:----------------------------:|
|log in to a remote system via SSH: `$ ssh user@host`    | login, interactive	 	|
|execute a script remotely: `$ ssh user@host 'echo $PWD'`| non-login, non-interactive |
|execute a script remotely and request a terminal: `$ ssh user@host -t 'echo $PWD'`|non-login, interactive |


# Some common operations and shell modes

|Operation                                               |Shell modes			    |
| ----------------------------------|:------------------------:|
|start a new shell process: `$ bash`                     |non-login, interactive    |
|run a script: `$ bash myscript.sh`                      |non-login, non-interactive|
|run an executable with `#!/usr/bin/env` (shebang)    |non-login, non-interactive|
|open a new graphical terminal window/tab on Mac OS X    |login, interactive		|
|open a new graphical terminal window/tab on Unix/Linux  |non-login, interactive    |


# Shell initialization files

-  A session of a **login shell** will first read configuration details
from `/etc/profile`.
	- It then reads the first file that it can find out of:
	- `~/.bash_profile`
	- `~/.bash_login`
	- `~/.profile` 
-  A session defined as a **non-login shell** will read (system)`/etc/bash.bashrc` and then the user-specific `~/.bashrc` file to build its environment.
- **Non-interactive shells** read the environmental variable called
`BASH_ENV` and read the file specified to define the new
environment.

# Shell initialization files

- **Exercise**: make a listing of all bash-related initialization files on your system.
	- you have to find out how to list so-called *hidden* files (i.e., entries starting with)
	- `man` command is your friend here

# Shell initialization files

- Recalling the functionality of `PATH` to contain a list (separated by `:`) of all directories in the system where to search for executable files
  ```bash
  $ echo $PATH
  -> /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
  ```
  - Usually, distributions install software in one of these directories
- Lets assume we have a directory `/opt/someprogram/bin` containing the executable of an externally installed program and want to add it to `PATH`
  ```bash
  $ export PATH="$PATH:/opt/someprogram/bin"
  $ echo $PATH
  -> /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/opt/someprogram/bin
  ```

# Aliases

- An alias is a (usually short) name that the shell translates into
another (usually longer) name or command.
-  Aliases allow you to define new commands by substituting a string
for the first token of a simple command.
- They are typically placed in the `~/.bashrc` file so that they are
available to interactive subshells.

# Aliases

- The general syntax for the alias command varies somewhat according to the type of shell. 
- In the case of `bash` shell it is
  ```bash
  $ alias [name='value']
  ```
  - `name` is the name of the new alias
  - `value` is the command which the alias will initiate.
  - the alias name and the replacement text can contain any valid shell input except for `=` (the equals sign).
  - if run without any argument, `alias` displays current set aliases	

# Aliases

- An example of alias creation could be the alias `p` for the commonly used `pwd` command:
  ```bash
  $ alias p="pwd"
  ```
-  An alias can be created with the same name as the corresponding command:
   ```bash
   $ alias ls="ls --color=auto"
   ```
   - From the shell, `ls` calls this alias
   - Disable it temporarily by preceding it with a backslash:
   ```bash
   $ \ls
   ```
   - An alias does not replace itself, avoiding infinite recursion.
	
# Aliases

- **Exercise**: You remember from the first session that the `-i` option of the remove command `rm` adds a level of security to to it. Create an alias in the shell of your appliance that redefines `rm` to include its own `-i` option. 

# Aliases

- You can nest aliases:
  ```bash
  $ alias l="ls -1"
  $ alias lc="l | wc -l"
  ```
  - Now you can even change the alias for `l` and have the changed behaviour in alias `lc`, too.
- Use the unalias built-in to remove an alias:
  ```bash
  $ unalias l lc
  ```
  - MIND: Aliases are disabled for non-interactive shells (that is, shell
scripts); you have to use the actual commands instead.


# Changing your own .bashrc

- **Exercise**: 
	1. Create a sub-directory `bin` in your home-directory 
	2. From the earlier exercise, copy or move the executable script `befriednly.sh` 
	3. In your `.bashrc`, add the subdirectory `$HOME/bin` to your existing `PATH` variable (be sure not to overwrite it!)
	4. Open a new terminal (or do a `$ source ~/.bashrc`)	
	5. Execute 
	```bash
	$ befriendly.sh
	```
	Does it work?

# Changing your own .bashrc

- **Exercise** (continued): 
	6. make an alias entry that prevents accidentally overwriting existing files with the `mv` command into `.bashrc`
	7. Copy the contents of your final `.bashrc` into Moodle
	   


