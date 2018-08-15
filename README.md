# Archfs: (A)rch Linux (F)rom (Scratch)

Arch Linux is cool, but it uses systemd. In this tutorial, I will show
how to install Arch Linux from scratch - a very clean setup with pacman
used as a package manager, and without systemd.

## 1. Download ArchLinux Live Disk

You need a live cd-rom to continue.

Boot into it and expand the live image:
$ mount -o remount,size=8G /run/archiso/cowspace

Execute wifi-menu
$wifi-menu

## 2. Create System Partition

Use the live cd-rom to create an ext4fs partition for the new
system:

- UEFI System Partition
- BIOS System Partition
- Linux swap partition
- Linux system
....
- Linux /home partion

## 3. Mount the target system

$ mount /dev/sda4 /mnt

$ mkdir /mnt/boot
$ mkdir /mnt/tmp
$ mkdir /mnt/run
$ mkdir /mnt/dev
$ mkdir /mnt/sys
$ mkdir /mnt/proc 
$ mkdir /mnt/home

$ mount /dev/sda1 /mnt/boot
$ mount /dev/sda7 /mnt/home
$ swapon /dev/sda3

$ mount --bind /tmp /mnt/tmp
$ mount --bind /run /mnt/run
$ mount --bind /dev /mnt/dev
$ mount --bind /sys /mnt/sys
$ mount --bind /proc /mnt/proc

## 4. Installing pacman database

$ mkdir -p /mnt/var/lib/pacman
$ pacman -Sy -r /mnt

## 5. Install pseudo systemd

### makepkg dependencies
$ pacman -Sy fakeroot binutils

### go to tmp
$ cd /tmp

### systemd
$ mkdir systemd
$ chmod a+rw systemd
$ cd systemd
$ nano PKGBUILD
pkgname=('systemd' 'libsystemd' 'systemd-resolvconf' 'systemd-sysvcompat')
pkgver=239.0
pkgrel=2
arch=('any')
package_systemd() {
    echo;
}
package_libsystemd() {
    echo;
}
package_systemd-resolvconf() {
    echo;
}
package_systemd-sysvcompat() {
    echo;
}
$ su -s /bin/bash nobody makepkg
$ pacman -Ur /mnt systemd*.xz libsystemd*.xz
$ cp systemd*.xz libsystemd*.xz /mnt/var/lib/pacman/
$ cd ..

### libsystemd.so

TODO

## 6. Installing base system

BTW, while you are installing these packages (below), you can pause at any time and chroot to /mnt to see 
the fruits of your labor.

### Filesystem tree
$ pacman -Sr /mnt iana-etc
$ pacman -Sr /mnt tzdata
$ pacman -Sr /mnt filesystem

### C library
$ pacman -Sr /mnt linux-api-headers
$ pacman -Sr /mnt glibc
$ pacman -Sr /mnt gcc-libs

### shell
$ pacman -Sr /mnt ncurses
$ pacman -Sr /mnt readline
$ pacman -Sr /mnt bash

### POSIX capabilities
$ pacman -Sr /mnt attr
$ pacman -Sr /mnt acl
$ pacman -Sr /mnt libcap
$ pacman -Sr /mnt libcap-ng

### Perl language
$ pacman -Sr /mnt db
$ pacman -Sr /mnt gdbm
$ pacman -Sr /mnt perl

### Filesystem
$ pacman -Sr /mnt libutil-linux
$ pacman -Sr /mnt e2fsprogs
$ pacman -Sr /mnt dosfstools

### Crypto
$ pacman -Sr /mnt gmp
$ pacman -Sr /mnt openssl
$ pacman -Sr /mnt openssl-1.0
$ pacman -Sr /mnt libsasl
$ pacman -Sr /mnt libgpg-error
$ pacman -Sr /mnt libgcrypt
$ pacman -Sr /mnt nettle

### Compression
$ pacman -Sr /mnt zlib
$ pacman -Sr /mnt bzip2
$ pacman -Sr /mnt lz4
$ pacman -Sr /mnt xz
$ pacman -Sr /mnt pcre
$ pacman -Sr /mnt less
$ pacman -Sr /mnt gzip
$ pacman -Sr /mnt zip

## Archiving
$ pacman -Sr /mnt expat
$ pacman -Sr /mnt tar
$ pacman -Sr /mnt libarchive

### Coreutils
$ pacman -Sr /mnt coreutils
$ pacman -Sr /mnt findutils

### Binutils
$ pacman -Sr /mnt libelf
$ pacman -Sr /mnt binutils

### glib2
$ pacman -Sr /mnt libffi
$ pacman -Sr /mnt glib2

### Text processing
$ pacman -Sr /mnt mpfr
$ pacman -Sr /mnt gawk
$ pacman -Sr /mnt grep
$ pacman -Sr /mnt sed

### Passwords
$ pacman -Sr /mnt keyutils
$ pacman -Sr /mnt cracklib

### Kerberos
$ pacman -Sr /mnt libldap
$ pacman -Sr /mnt krb5
$ pacman -Sr /mnt libtirpc
$ pacman -Sr /mnt libnsl

### User management
$ pacman -Sr /mnt pambase
$ pacman -Sr /mnt pam
$ pacman -Sr /mnt shadow

### more utility commands
$ pacman -Sr /mnt util-linux
$ pacman -Sr /mnt diffutils
$ pacman -Sr /mnt procps
$ pacman -Sr /mnt psmisc
$ pacman -Sr /mnt sudo

### sqlite
$ pacman -Sr /mnt sqlite
$ pacman -Sr /mnt yajl

### PKCS modules
$ pacman -Sr /mnt libtasn1
$ pacman -Sr /mnt p11-kit

### Certificates
$ pacman -Sr /mnt ca-certificates-utils
$ pacman -Sr /mnt ca-certificates-cacert
$ pacman -Sr /mnt ca-certificates-mozilla
$ pacman -Sr /mnt ca-certificates

### internationalization
$ pacman -Sr /mnt perl-xml-parser
$ pacman -Sr /mnt libunistring
$ pacman -Sr /mnt intltool
$ pacman -Sr /mnt gettext

### Domain Names
$ pacman -Sr /mnt libidn
$ pacman -Sr /mnt libidn2
$ pacman -Sr /mnt libpsl

### Web fetching
$ pacman -Sr /mnt libssh2
$ pacman -Sr /mnt libnghttp2
$ pacman -Sr /mnt curl

### GNU TLS
$ pacman -Sr /mnt gnutls

### Assuan Protocol
$ pacman -Sr /mnt libassuan
$ pacman -Sr /mnt libsecret
$ pacman -Sr /mnt pinentry

### GNU PG
$ pacman -Sr /mnt libksba
$ pacman -Sr /mnt npth
$ pacman -Sr /mnt gnupg
$ pacman -Sr /mnt gpgme

### Pacman
$ pacman -Sr /mnt fakeroot
$ pacman -Sr /mnt archlinux-keyring
$ pacman -Sr /mnt pacman-mirrorlist
$ pacman -Sr /mnt pacman
$ pacman -Sr /mnt arch-install-scripts

### Text Editor
$ pacman -Sr /mnt file
$ pacman -Sr /mnt nano
$ pacman -Sr /mnt gpm
$ pacman -Sr /mnt vim-runtime
$ pacman -Sr /mnt vim

### Message passing
$ pacman -Sr /mnt dbus

### USB
$ pacman -Sr /mnt hwids
$ pacman -Sr /mnt libusb
$ pacman -Sr /mnt usbutils

### PCI
$ pacman -Sr /mnt kbd 
$ pacman -Sr /mnt kmod
$ pacman -Sr /mnt pciutils

### Packet capturing
$ pacman -Sr /mnt libnl
$ pacman -Sr /mnt libpcap

### Network
$ pacman -Sr /mnt libmnl
$ pacman -Sr /mnt libnftnl
$ pacman -Sr /mnt iptables
$ pacman -Sr /mnt iproute2
$ pacman -Sr /mnt iputils
$ pacman -Sr /mnt inetutils
$ pacman -Sr /mnt net-tools
$ pacman -Sr /mnt wpa_supplicant

### Internet
$ pacman -Sr /mnt dhclient
$ pacman -Sr /mnt wget
$ pacman -Sr /mnt lynx

### GNU Package Conf tools
$ pacman -Sr /mnt m4
$ pacman -Sr /mnt autoconf 
$ pacman -Sr /mnt automake 
$ pacman -Sr /mnt pkg-config 
$ pacman -Sr /mnt libtool
$ pacman -Sr /mnt which

### C/C++ Compiler
$ pacman -Sr /mnt libmpc
$ pacman -Sr /mnt gcc
$ pacman -Sr /mnt libatomic_ops
$ pacman -Sr /mnt gc

### make utility
$ pacman -Sr /mnt texinfo
$ pacman -Sr /mnt guile
$ pacman -Sr /mnt make

### python
$ pacman -Sr /mnt python2
$ pacman -Sr /mnt python3
$ pacman -Sr /mnt python2-pip
$ pacman -Sr /mnt python-pip

### ruby
$ pacman -Sr /mnt libyaml
$ pacman -Sr /mnt ruby rubygems

### NetworkManager
$ pacman -Sr /mnt networkmanager

### git
$ pacman -Sr /mnt git

### alsa
$ sudo pacman -S moc alsaplayer alsa-tools alsa-utils alsa-firmware

## 7. Chrooting to the installed system

# chroot
$ chroot /mnt /usr/bin/bash

# connect to the internet in chroot'd env.
$ nano /etc/resolv.conf
nameserver 8.8.8.8

# generate locale
$ nano /etc/locale.gen
$ locale-gen

# initialize pacman
$ nano /etc/pacman.d/mirrorlist
$ pacman-key --init
$ pacman-key --populate archlinux

# users
$ groupadd users
$ useradd -s /usr/bin/bash -d /home/iocoder -m iocoder
$ passwd root
$ passwd iocoder
$ nano /etc/sudoers
$ su iocoder
$ cd ~

# install package-query (for yaourt)
$ wget https://aur.archlinux.org/cgit/aur.git/snapshot/package-query.tar.gz
$ tar -xzf package-query.tar.gz
$ cd package-query
$ makepkg
$ sudo pacman -U package-query*.xz
$ cd ..
$ rm -rf package-query*

# install yaourt
$ wget https://aur.archlinux.org/cgit/aur.git/snapshot/yaourt.tar.gz
$ tar -xzf yaourt.tar.gz
$ cd yaourt
$ makepkg
$ sudo pacman -U yaourt*.xz
$ cd ..
$ rm -rf yaourt*

# now use yaourt to install eudev
$ yaourt eudev

# generate fstab
$ sudo su
$ genfstab / > /etc/fstab
$ cat /etc/fstab
$ exit

# procps-ng-nosystemd
$ yaourt procps-ng-nosystemd

# dbus-nosystemd:
$ yaourt dbus-nosystemd
$ sudo su
$ dbus-uuidgen > /etc/machine-id
$ useradd avahi
$ groupadd network
$ exit

## 8. Installing the kernel!

### Installing mkinitcpio
$ sudo pacman -S mkinitcpio-busybox
$ sudo pacman -S mkinitcpio

### Installing linux
$ sudo pacman -S linux-firmware
$ sudo pacman -S linux
$ sudo pacman -S linux-headers

### Creating your own init script!
$ sudo nano /bin/init
#!/bin/sh

echo Hello from my init daemon!
exec bash
$ sudo chmod +x /bin/init

## 9. Rebooting
$ exit
$ exit
$ reboot

ENJOY YOUR SYSTEM!
