---
title:  Text Editors in the shell
author: CSC Training
date:     2020-04
lang:     en
---
# Contents of this session

This session shall introduce you to the basics of the following UNIX/Linux text editors

1. nano
2. emacs
3. vi

# Why text editors?

- Several times in this course you encountered the phrase "_everything is text_".  Consequently, it is important to create and manipulate text in files
	- We discuss standard (shell) text editors you most likely can expect to be found on Linux computers by default
	- some applications use these text editors as defaults (e.g. `git` on Ubuntu calls `nano` for writing commit messages)
- For users being used to editors or text processing programs from Mac or Windows, these editors may not immedeately feel intuitive
	- Alternatively, there are editors in Linux desktop GUIs that are closer to Windows way of usage, e.g., gedit or Kate
    - for instance, gedit supports **ctrl+c** (copying), **ctrl+x** (cut) and **ctrl+v** (Paste)
	- No rules here: use, what suits you best
	
# Nano — a basic editor for terminal

- For opening and creating files, type:
```bash
$ nano filename
```
     - If editing a configuration file use the `-w` switch to disable wrapping on long lines and avoid consequent errors in parsing:
```bash
$ nano -w /etc/fstab
```
	- If you want to save the changes you made, press **ctrl+o** (`^O`)
		- NB: this combination often is denoted using the carrot symbol **`^`** to stand for **ctrl**, i.e., `^O` 
	- To exit nano, type **ctrl+x** (`^X`).
		- In case of unsaved modifications, nano will ask you if you want to save it. Answer either  **n** (no)  or **y** (yes) 
		- In the latter case, nano prompts for a filename - type it in and press **Enter**

# Nano — a basic editor for terminal

- **Editing** with nano:
	- cutting single line: **ctrl+k** (`^K`) 
	- pasting cutted line: **ctrl+u** (`^U`)
	- placing marker: **ctrl+6** (`^6`), then move cursor forward and hit `^K` to cut text under highlighted region
- **Searching** with nano:
	- search is activated with **ctrl+w** (`^W`); then type in string followed by **Enter**
	- reoccuring instances of same string can be found with **alt+w** (`M-W`) 
	  - `M-` stands for meta-key, usally the **alt** key
	
