# Operating system practice

## What is the project mainly about?

### This project consists of three parts.
- The first part is to build the linux kernel for TI OMAP 5912.


## What am I going to do?
- Build cross development toolchain
- Build linux kernel\
&rarr; Check Point 1: uImage
- Setup tftp server
- Setup NFS server
&rarr; Check Point 2: test the services
- Setup target board
- Download linux kernel
&rarr; Check Point 3: test the linux kernel

## Fedora Linux
- The Fedora Project was created in late 2003.
- I'm using the version 38.
- Package manager: RPM
- Update method: yum
  - Password: csie123456
  - Select the language: Taiwan
  - WindowsKey + space to change the input language
  - Activities&rarr; Search: terminal&rarr; to get the terminal
    - Edit&rarr; Profile Preferences&rarr; Colors&rarr; Uncheck "use colors from system theme"
  - Click the icon at the right-top corner for network setting

## Setting Network

## vi--A Screen-Oriented Text Editor
- vi is widely supported by Unix-like operating system.
- Normal mode:
  - Move, search, copy, paste, delete,...
  - Press i, I, a, A, o, O,... to change to the insert mode
  - Press : for the command mode
- Command mode:
  - Save, quit, load, split, ...
  - After enter the command, it will back to the normal mode
- Insert mode:
  - Move & input anything
  - Press ESC to go back to the normal mode

## vi Commands
  - Press ‘i’ to get the insert mode
  - Key-in anything
  - Press ‘ESC’ to go back to the normal mode
  - Press ‘:->w->q->ENTER’ to save & quit
  - Please search for some trivial of vi & study by yourself

## Part I: Build the Linux Kernel for TI OMAP 5912.

### Download files
  - Download the tools from the course website and extract the files
  - I will need the following files
    - linux-2.6.12-omap.tgz-> the kernel source code
    - gcc-3.3.2.tar-> some gcc extension for this project
    - mkimage-> some script which is used when compiling kernel
    - rootfsosk.tar.bz2-> the content of the root filesystem
  - I will need root privilege for the following actions.
    - *sudo passwd root*

