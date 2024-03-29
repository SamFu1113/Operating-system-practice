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
    - *su* (the password is root123456)-> change to root
    - *cd /home/csie/LAB1*
    - *cp linux-2.6.12-omap.tgz /opt/linux-2.6.12-omap.tgz*
    - *cp gcc-3.3.2.tar /opt/gcc-3.3.2.tar*
    - *chmod +x mkimage*
    - *cd /opt*
    - *tar xvf gcc-3.3.2.tar*
    - *tar zxvf linux-2.6.12-omap.tgz*
    - *cp home/csie/LAB1/mkimage /opt/usr/local/arm/3.3.2/bin/mkimage*

### Prepare the Compiling Environment
  - Set Path
    - *export PATH=$PATH:/opt/usr/local/arm/3.3.2/bin*-> for every terminal session, before I compile the kernel
    - *export LANG=en*
  - Install Tools
    - *yum -y install gcc*-> compiler tools
    - *yum -y install glibc.i686*-> library for 32-bit Linux Kernel
    - *yum -y install minicom*-> minicom is the utility for the serial port connection

### Build the Linux Kernel
  - Go to the kernel source directory (be the root)
     - *cd /opt/linux-2.6.12*
  - Set the kernel configuration
     - *make omap_osk_5912_deconfig*
  - Compile the kernel
     - *make uImage*
  - Prepare the root filesystem
     - *cp /home/csie/LAB1/rootfsosk.tar.bz2 /tmp/rootfsosk.tar.bz2*
     - *cd /tmp*
     - *tar jxvf rootfsosk.tar.bz2*

### Check Point 1
  - Now, I should have the compiled kernel
  - The kernel imags is at: /opt/linux-2.6.12/arch/arm/boot/uImage
  - The root filesystem for the evaluation board is at: /tmp/roorfs2.6

### Set the Network Services (1/2)
  - Disable the Firewall (it is not a good idea, only for this lab exercise)
    - *systemctl stop firewalld*
    - *systemctl disable firewalld*
  - Set the TFTP Service
    - ~~yum -y install tftp-server tftp-> tftp is used to download kernel image~~
  - ~~vi /etc/xinetd.d/tftp~~
    - ~~Find disable = yes~~
    - ~~Change it to disable = no~~
  - ~~systemctl start tftp.socket~~
  - ~~systemctl enable tftp.socket~~
  - *dnf install tftp-server tftp -y*
  - *cp /usr/lib/systemd/system/tftp.service /etc/systemd/system/tftp-server.service*
  - *cp /usr/lib/systemd/system/tftp.socket /etc/systemd/system/tftp-server.socket*
  - *vi /etc/systemd/system/tftp-server.service*
  - *systemctl daemon-reload*
  - *systemctl enable --now tftp-server*
  - *chmod 777 /var/lib/tftpboot*
