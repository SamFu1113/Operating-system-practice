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

### Set the Network Services (2/2)
  - Set the NFS Service
    - *yum -y install nfs-utils*-> nfs for the root filesystem
    - *vi /etc/exports*
      - Add the line /tmp/rootfs2.6 *(rw,fsid=1,no_root_squash)
    - *vi /etc/nfs.conf*
      - Add the line udp=y
    - exportfs -rv
    - systemctl start rpcbind.service
    - systemctl start nfs-mountd.service

### Test the Network Services
  - I need a friend for the following test
    - One be the server and the other be the client
    - Switch the roles & do it again
  - Test TFTP
    - Server side:
      - *vi /var/lib/tftpboot/testfile*-> and then key something
    - Client side:
      - *tftp 192.168.68.xxx* (xxx is for the server IP)
      - *get testfile*
      - *quit*
      - *cat testfile*
  - Test NFS
    - Server side:
    - Client side:
      - *mkdir /home/csie/nfstest*
      - *mount -t nfs 192.168.68.xxx:/tmp/rootfs2.6 /home/csie/nfstest*
      - *cd /home/csie/nfstest*
      - *ls*
      - *cd /*
      - *umount /home/csie/nfstest*
     
### Check Point 2
  - Now, I have enabled the TFTP & NFS services on my PC
  - TFTP & NFS are properly working now 
  
### Set the Minicom (1/3)
  - Enter the setting menu
    - *minicom -s*
  - Serial port setup-> press the letter to change it

### Set the Minicom (2/3)
  - Modem & dialing

### Set the Minicom (3/3)
  - Save & leave the setting interface
  - Start & quit minicom
    - Start *minicom*
    - Quit *CTRL + A -> Q*

### Prepare for the Booting
  - Copy the boot image for TFTP booting
    - *cp /opt/linux-2.6.12/arch/arm/boot/uImage /var/lib/tftpboot/uImage*
  - Set the evaluation board as follows

# Boot the Evaluation Board
  - Start the minicom
    - *minicom -s*
  - Press the reset button on the board
    - After the reset, immediately press any key on minicom terminal
    - I will get the following prompt

### Download the New Kernel
  - Set the boot configuration
    - *set ipaddr 192.168.68.yy* (evaluation board IP)
    - *set serverip 192.168.68.zz* (PC IP)
    - *set netmask 255.255.255.0*
    - *set gatewayip 192.168.68.254*
    - *set ethaddr 00-0e-99-xx-xx-xx*
    - *set bootargs console=ttyS0, 115200n8 rw ip=192.168.68.yy root=/dev/nfs nfsroot=192.168.68.zz:/tmp/rootfs2.6,v3*
    - *printenv*->double check the setting
   
### Boot the New Kernel and Mount the NFS Root Filesystem
  - Download the kernel: *tftpboot 0x10000000 uImage*
  - Boot the OS: *bootm 0x10000000*

### Done!
### Or Bugs!?

### Common Mistakes
  - *su* & *export* should be used whenever a new terminal is created
    - If I extract the root file system by the user csie, there will be an error when I boot the board to mount the NFS root file system
      - Reboot the computer & do everything again
    - If I do not export the path of the tools, I will get some error when I compile the kernel module
  - Please read the error message if I type something wrong
  - UART: it should be connected to the bottom port
  - Ethernet: do check the IP is correct
  - Some evaluation boards were tested to be good: (no USB cable), 7, 9, 10, 11, 15, 19, 20

### Grading this Exercise
  - Attend and understand this exercise: 10%
  - Check point 1: 10%
  - Check point 2: 10%
  - Final result: 20%
  - Report before the exercise: 25%
  - Report after the exercise: 25%

### Report Requirements
  - Report before the exercise:
    - Only two pages, 12-pt font size
    - Deadline is 20:00, 2023/05/17
    - File name: OSP-Lab1-Study-StudentID
    - File type: PDF or Word
    - Upload to t
