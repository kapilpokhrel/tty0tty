# tty0tty

_linux null modem emulator v1.3_


## Directory tree

This is the tty0tty directory tree:

* `module` : linux kernel module null-modem
* `pts`    : null-modem using ptys (without handshake lines)
* `debian` : debian package build tree


### pts (unix98)

When run connect two pseudo-ttys and show the connection names:

    (/dev/pts/1) <-> (/dev/pts/2) 

the connection is:

    TX -> RX
    RX <- TX

### module

The module is tested in kernels from 3.10.2 to 5.3.15 (debian) 

When loaded, create 8 ttys interconnected:

    /dev/tnt0  <->  /dev/tnt1 
    /dev/tnt2  <->  /dev/tnt3 
    /dev/tnt4  <->  /dev/tnt5 
    /dev/tnt6  <->  /dev/tnt7 

the connection is:

    TX   ->  RX
    RX   <-  TX
    RTS  ->  CTS
    CTS  <-  RTS
    DSR  <-  DTR
    CD   <-  DTR
    DTR  ->  DSR
    DTR  ->  CD
  

## Build from source


for module build is necessary kernel-headers or kernel source:

    sudo apt-get  update
    sudo apt-get  install linux-headers-`uname -r`
    cd module
    make
    sudo make install

for debian package:

    sudo apt-get install devscripts build-essential lintian

Download the archive file from https://github.com/lcgamboa/tty0tty/archive/refs/heads/master.zip

    wget https://github.com/lcgamboa/tty0tty/archive/refs/heads/master.zip
    unzip master.zip
    cd tty0tty-master
    dpkg-buildpackage

to clean:

    dh_clean

for pts build:

    cd pts
    make

then run with:

    ./tty0tty

`make install` set the devices permissions automatically in udev creating the file /etc/udev/rules.d/99-tty0tty.rules with rules:

    SUBSYSTEM=="tty", KERNEL=="tnt0", GROUP="dialout", MODE="0660"
    SUBSYSTEM=="tty", KERNEL=="tnt1", GROUP="dialout", MODE="0660"
    SUBSYSTEM=="tty", KERNEL=="tnt2", GROUP="dialout", MODE="0660"
    SUBSYSTEM=="tty", KERNEL=="tnt3", GROUP="dialout", MODE="0660"
    SUBSYSTEM=="tty", KERNEL=="tnt4", GROUP="dialout", MODE="0660"
    SUBSYSTEM=="tty", KERNEL=="tnt5", GROUP="dialout", MODE="0660"
    SUBSYSTEM=="tty", KERNEL=="tnt6", GROUP="dialout", MODE="0660"
    SUBSYSTEM=="tty", KERNEL=="tnt7", GROUP="dialout", MODE="0660"

It's possible edit rules and create permanent symbolic names with the parameter SYMLINK:

    SUBSYSTEM=="tty", KERNEL=="tnt0", GROUP="dialout", MODE="0660", SYMLINK+="ttyUSB10"
    SUBSYSTEM=="tty", KERNEL=="tnt1", GROUP="dialout", MODE="0660", SYMLINK+="ttyUSB11"


The user must be in the dialout group to access the ports. 
To add your user to dialout group use the command:
 
    sudo usermod -a -G dialout your_user_name

after this is necessary logout and login to group permissions take effect.


To dkms support use the scripts `dkms-install.sh` and  `dkms-remove.sh`


For e-mail suggestions :  lcgamboa@yahoo.com
