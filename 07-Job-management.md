---
title:  Job management
author: CSC Training
date:     2020-04
lang:     en
---
# Contents of this session

UNIX/Linux are multi-tasking operating systems. Consequently, the users need to know something about managing their jobs (=running applications). This session deals with ...

- displaying jobs on your system
- fore- and background jobs in shell
- suspending and terminating of jobs

# Jobs on system

- Linux/UNIX are multi-tasking systems
- One can display the currently running jobs using the `top` command
  ```bash
  $ top
  ```
  - press `P` to order jobs by their processor load
  - press `M` to order jobs by their memory consumption
  - press `u` to prompt for displaying jobs of certain user (enter `cscuser`)
  - press `q` to exit

# Fore- and background

- By default commands (jobs) in the shell are run in foreground, e.g.,
  ```bash
  $ emacs newfile
  ```
- Now, try to enter something in your shell
	- It does not respond. Why?
	- as long as you do not quit `emacs` (the currently running program), it blocks the shell
	- we say: the job (`emacs` in this case) is running in the **foreground** of the shell
- **Terminating** (=killing) a running foreground job: in shell press **ctrl + C**
	- That is usually not recommended, except for non responding jobs
	
# 	Managing jobs

- Launch again into foreground
	- Type something into `emacs`
- **Suspending** a job: in shell press **ctrl + z**
	- Shell reports on stopped job
- Type a command into the shell: 
  ```bash
  $ ls -ltr
  ```
  -  Try to type something into emacs. What happens?
	 - The process of `emacs` is suspended, hence does not accept any input
	   - In fact, the input buffer keeps the typed stuff and will fill it into `emacs`, once it is active again
	   
# Managing jobs
- **Sending** the suspended job **to background**:
  ```bash
  $ bg
  ````
  - type a command into the shell: 
  ```bash
  $ ls -ltr
  ```
  - type something into emacs
  - It works now for both! Because `emacs` is running in background and your shell accepts commands
	
# Managing jobs

- **Fetching** back **to foreground**:
  ```bash
  $ fg
  ```
  - Shell is blocked again
  - `emacs` accepts input 
  - quit `emacs`  either by menu or **ctrl+x, ctrl+c**	
- Can I directly **launch into background** ?
  - Yes. Type command followed by **`&`**
  ```bash
  $ emacs newfile &
  ```
	
# Managing jobs

- Lets launch two jobs into background
  ```bash
  $ xterm -T "first" &
  $ xterm -T "second" &
  ```
  - if program `xterm` is not installed, install it
- To **list** all **jobs** of the shell:
  ```bash
  $ jobs
  ```
  should report something like (numbers in brackets might differ)
  ```bash
  [1] - Running      xterm -T "first" &
  [2] + Running      xterm -T "second" &
  ```
  - the `+` after job `[2]` means that job `[2]` listens to commands, like `fg`
	
# Managing jobs

- Bringing job `[1]` to foreground needs a way to **address the job**:
  ```bash
  $ fg %1
  -> xterm -T "first"
  ```
  - address a certain job with  `%` followed by its number
  - to send it back **ctrl + z**, followed by
  ```bash
  $ bg
  ```
	
# Managing jobs

- To kill (= terminate) a certain job
  ```bash
  $ kill -9 %1
  $ jobs
  -> [1]-  Killed                  xterm -T "first"
  [2]+  Running                 xterm -T "second" &
  ```
- Playing terminator (killing all jobs of a kind):
  ```bash
  $ for i in {1..5}; do xterm & done
  $ pkill xterm
  ```
	- this first launches 5 xterms and then kills all xterm processes under your user-id
	

   
	



  

