# Buildroot-Linux-with-gcc_cross_compilation-for-Raspberry-Pi-3
This repository documents my process of building a minimal Linux image for Raspberry Pi 3 using **Buildroot**.  
The goal is to understand Linux system components like BusyBox, rootfs, and bootloaders through hands-on experimentation.

The system includes:
- A minimal Linux kernel  
- BusyBox utilities  
- SSH for remote access  
- Basic networking setup

##  Project Details

| Item | Description |
|------|--------------|
| **Target Board** | Raspberry Pi 3 |
| **Build System** | Buildroot |
| **Filesystem Type** | ext4 |
| **Default Config Used** | raspberrypi3_defconfig |
| **Access Method** | SSH |

## Boot Flow 
 **Power On** ⇨ **Bootloader** (loads kernel +dtb)  ⇨ **Kernel** (initiates hardware) ⇨ **Mounts** (rootfs) ⇨ **Runs** (BusyBox) ⇨ **You get a shell** 

## Installation Steps

### Install Prerequisites
```sh
$ sudo apt update
$ sudo apt install which sed make binutils build-essential \ diffutils gcc g++ bash patch gzip bzip2 perl tar cpio \ unzip rsync file bc findutils wget python3 libncurses5-dev \ libncursesw5-dev git
```
### Download Buildroot
```sh
$ git clone https://github.com/buildroot/buildroot.git
```
### Select the Board
```sh
$ make raspberrypi3_defconfig
```
### Customize the Build
```sh
$ make menuconfig
$ make linux-menuconfig
$ make busybox-menuconfig
```
### Enable the Root Password 
### Build the system
```sh
$ make
```
### Flash the image to SD Card using
```sh
$ sudo dd if=output/images/sdcard.img of=/dev/sdX bc=4M
``` 
### Booted on Raspberry Pi 3 and accessed via SSH
```sh
$ ssh root@10.247.xx.xx
```
### Output Terminal 
```sh
$ root@10.247.xx.xx's password:

# uname -a
Linux buildroot 6.12.41-v7 #1 SMP Fri Oct 10 20:03:22 IST 2025 armv7l GNU/Linux

# hostname
buildroot

# busybox
BusyBox v1.37.0 (2025-10-10 19:54:00 IST) multi-call binary.
BusyBox is copyrighted by many authors between 1998-2015.
Licensed under GPLv2. See source distribution for detailed
copyright notices.

Usage: busybox [function [arguments]...]
   or: busybox --list[-full]
   or: busybox --show SCRIPT
   or: busybox --install [-s] [DIR]
   or: function [arguments]...

	BusyBox is a multi-call binary that combines many common Unix
	utilities into a single executable.  Most people will create a
	link to busybox for each function they wish to use and BusyBox
	will act like whatever it was invoked as.

Currently defined functions:
	[, [[, addgroup, adduser, ar, arch, arp, arping, ascii, ash, awk, base32, base64, basename, bc, blkid, bunzip2, bzcat, cat, chattr,
	chgrp, chmod, chown, chroot, chrt, chvt, cksum, clear, cmp, cp, cpio, crc32, crond, crontab, cut, date, dc, dd, deallocvt, delgroup,
	deluser, devmem, df, diff, dirname, dmesg, dnsd, dnsdomainname, dos2unix, du, dumpkmap, echo, egrep, eject, env, ether-wake, expr,
	factor, fallocate, false, fbset, fdflush, fdformat, fdisk, fgrep, find, flock, fold, free, freeramdisk, fsck, fsfreeze, fstrim, fuser,
	getfattr, getopt, getty, grep, gunzip, gzip, halt, hdparm, head, hexdump, hexedit, hostid, hostname, hwclock, i2cdetect, i2cdump,
	i2cget, i2cset, i2ctransfer, id, ifconfig, ifdown, ifup, inetd, init, insmod, install, ip, ipaddr, ipcrm, ipcs, iplink, ipneigh,
	iproute, iprule, iptunnel, kill, killall, killall5, klogd, last, less, link, linux32, linux64, linuxrc, ln, loadfont, loadkmap, logger,
	login, logname, losetup, ls, lsattr, lsmod, lsof, lspci, lsscsi, lsusb, lzcat, lzma, lzopcat, makedevs, md5sum, mdev, mesg, microcom,
	mim, mkdir, mkdosfs, mke2fs, mkfifo, mknod, mkpasswd, mkswap, mktemp, modprobe, more, mount, mountpoint, mt, mv, nameif, netstat, nice,
	nl, nohup, nologin, nproc, nslookup, nuke, od, openvt, partprobe, passwd, paste, patch, pidof, ping, pipe_progress, pivot_root,
	poweroff, printenv, printf, ps, pwd, rdate, readlink, readprofile, realpath, reboot, renice, reset, resize, resume, rm, rmdir, rmmod,
	route, run-init, run-parts, runlevel, sed, seedrng, seq, setarch, setconsole, setfattr, setkeycodes, setlogcons, setpriv, setserial,
	setsid, sh, sha1sum, sha256sum, sha3sum, sha512sum, shred, sleep, sort, start-stop-daemon, strings, stty, su, sulogin, svc, svok,
	swapoff, swapon, switch_root, sync, sysctl, syslogd, tail, tar, tee, telnet, test, tftp, time, top, touch, tr, traceroute, tree, true,
	truncate, ts, tsort, tty, ubirename, udhcpc, uevent, umount, uname, uniq, unix2dos, unlink, unlzma, unlzop, unxz, unzip, uptime,
	usleep, uudecode, uuencode, vconfig, vi, vlock, w, watch, watchdog, wc, wget, which, who, whoami, xargs, xxd, xz, xzcat, yes, zcat

# ls /proc
1              158            17715          230            45             72             bus            irq            schedstat
108            15841          17731          232            47             74             cgroups        kallsyms       self
112            159            17734          24             48             75             cmdline        key-users      slabinfo
116            15974          17741          26             49             76             consoles       keys           softirqs
11636          16             17742          27             5              77             cpu            kmsg           stat
11655          161            17753          28             50             78             cpuinfo        kpagecgroup    swaps
12             16172          17754          29             51             79             crypto         kpagecount     sys
13             162            18             3              52             8              device-tree    kpageflags     sysrq-trigger
132            163            183            31             55             80             devices        latency_stats  sysvipc
14             164            19             32             56             81             diskstats      loadavg        thread-self
14049          17             2              33             57             82             driver         locks          timer_list
14889          171            20             34             6              83             execdomains    meminfo        tty
14954          17345          206            36             60             85             fb             misc           uptime
15             17388          21             4              61             86             filesystems    modules        version
152            17471          212            41             68             87             fs             mounts         vmallocinfo
15376          17596          22             42             69             88             interrupts     net            vmstat
157            17712          222            43             7              89             iomem          pagetypeinfo   zoneinfo
15723          17714          23             44             71             buddyinfo      ioports        partitions

# cat /proc/cpuinfo
processor	: 0
model name	: ARMv7 Processor rev 4 (v7l)
BogoMIPS	: 38.40
Features	: half thumb fastmult vfp edsp neon vfpv3 tls vfpv4 idiva idivt vfpd32 lpae evtstrm crc32 
CPU implementer	: 0x41
CPU architecture: 7
CPU variant	: 0x0
CPU part	: 0xd03
CPU revision	: 4

processor	: 1
model name	: ARMv7 Processor rev 4 (v7l)
BogoMIPS	: 38.40
Features	: half thumb fastmult vfp edsp neon vfpv3 tls vfpv4 idiva idivt vfpd32 lpae evtstrm crc32 
CPU implementer	: 0x41
CPU architecture: 7
CPU variant	: 0x0
CPU part	: 0xd03
CPU revision	: 4

processor	: 2
model name	: ARMv7 Processor rev 4 (v7l)
BogoMIPS	: 38.40
Features	: half thumb fastmult vfp edsp neon vfpv3 tls vfpv4 idiva idivt vfpd32 lpae evtstrm crc32 
CPU implementer	: 0x41
CPU architecture: 7
CPU variant	: 0x0
CPU part	: 0xd03
CPU revision	: 4

processor	: 3
model name	: ARMv7 Processor rev 4 (v7l)
BogoMIPS	: 38.40
Features	: half thumb fastmult vfp edsp neon vfpv3 tls vfpv4 idiva idivt vfpd32 lpae evtstrm crc32 
CPU implementer	: 0x41
CPU architecture: 7
CPU variant	: 0x0
CPU part	: 0xd03
CPU revision	: 4

Hardware	: BCM2835
Revision	: a22082
Serial		: 000000007c78add3
Model		: Raspberry Pi 3 Model B Rev 1.2

# ls /
bin           dev           lib           linuxrc       media         opt           root          sbin          tmp           var
crond.reboot  etc           lib32         lost+found    mnt           proc          run           sys           usr

# mount
/dev/root on / type ext4 (rw,relatime)
devtmpfs on /dev type devtmpfs (rw,relatime,size=426292k,nr_inodes=106573,mode=755)
proc on /proc type proc (rw,relatime)
devpts on /dev/pts type devpts (rw,relatime,gid=5,mode=620,ptmxmode=666)
tmpfs on /dev/shm type tmpfs (rw,relatime)
tmpfs on /tmp type tmpfs (rw,relatime)
tmpfs on /run type tmpfs (rw,nosuid,nodev,relatime,mode=755)
sysfs on /sys type sysfs (rw,relatime)

# df -h
Filesystem                Size      Used Available Use% Mounted on
/dev/root               107.0M     78.3M     20.3M  79% /
devtmpfs                416.3M         0    416.3M   0% /dev
tmpfs                   448.8M         0    448.8M   0% /dev/shm
tmpfs                   448.8M    292.0K    448.5M   0% /tmp
tmpfs                   448.8M     40.0K    448.8M   0% /run

# ls /etc
cron           fstab          hosts          issue          nsswitch.conf  profile        resolv.conf    shells
dhcpcd.conf    group          init.d         mtab           os-release     profile.d      services       ssh
dropbear       hostname       inittab        network        passwd         protocols      shadow         ssl
```

## Cross Compilation for GCC support
### Toolchain Setup
We use an external ARM toolchain located on the Desktop for cross-compilation.

(https://developer.arm.com/downloads/-/arm-gnu-toolchain-downloads)

### Check the host machine Configuration:
Download the toolchain refering to your host system configuration
```sh
~/Desktop$ gcc -dumpmachine
x86_64-linux-gnu
```
### Extract the Toolchain
```sh
$ tar -xvf gcc-arm-10.3-2021.07-x86_64-arm-none-linux-gnueabihf.tar.xz 
```
### Verify the Binary Path
Navigate to the bin directory to confirm the path
```sh
$ cd gcc-arm-10.3-2021.07-x86_64-arm-none-linux-gnueabihf/
$ pwd
```
### Configure the Environment Variable
The toolchain of your path will be temporarily added for current session
```sh
$ export PATH=/home/user/Downloads/gcc-arm-10.3-2021.07-x86_64-arm-none-linux-gnueabihf/bin:$PATH
```
Make it Permanent
```sh
$ echo 'export PATH=/home/user/Downloads/gcc-arm-10.3-2021.07-x86_64-arm-none-linux-gnueabihf/bin:$PATH' >> ~/.bashrc
$ source ~/.bashrc
```
### Verify the installation
Confirm the compiler version & compiler is accessible
```sh
~/Desktop$ arm-none-linux-gnueabihf-g++ --version
arm-none-linux-gnueabihf-g++ (GNU Toolchain for the A-profile Architecture 10.3-2021.07 (arm-10.29)) 10.3.1 20210621
Copyright (C) 2020 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE
```
### Create the test file
Example to Print hello on the Build machine
```sh
~/Desktop$ nano test.cpp

#include <iostream>

int main()
{
        std::cout<< "Hello.."<<std::endl;
        return 0;
        
}
```
### Compile the sketch
Use the -static flag to ensure the binary runs on the target without library conflicts.
```sh
~/Desktop$ arm-none-linux-gnueabihf-g++ -o test test.cpp -static
```
### Deployment via SCP
Deploy the compiled binary to the running Buildroot system:
```sh
~/Desktop$ scp ./test root@10.xxx.74.xx:/tmp
```
### Log in the board
Once you are logged into the board's terminal, navigate to the /tmp folder, make the file executable, and run it:
```sh
ssh root@10.xxx.xx.xx
# cd /tmp
# chmod +x test
# ./test
```
### Output
```sh
Hello..
```
