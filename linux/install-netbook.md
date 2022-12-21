# Install Netbook

Netbook Specs:
- Acer Aspire One D250-1042
- BIOS v1.29 (12/18/2010)
- Intel Atom N270 @ 1.60GHz (512KB L2 & 32KB L1)
- Hynix 2GB DDR2 533MHz SODIMM
- Kingston 480GB A400 SATA3 2.5" SSD (SA400S37/480G) (447GB formated space)
- Mobile 945GSE Express Memory Controller
- Mobile 945GM/GMS/GME, 943/940GML, Express Integrated Graphics Controller (VGA)
- Intel NM10/ICH7 Family High Definition Audio Controller
- Qualcomm Atheros AR242x/AR542x Wireless Network Adapter (PCI-Express)
- Qualcomm Atheros AR8132 Fast Ethernet 10/100 Mbit/s
- Intel 82801GBM/GHM (ICH7-M family) SATA Controller (AHCI mode)

This is simple Arch Linux 32bit install guide by Tuxy.

** Make Arch Linux 32bit USB **

** Boot Arch Linux From USB **

## Create Partition
_(create one big partition and make it bootable)_
```
# cfdisk
```

## Format Partition
```
# mkfs.ext4 /dev/sda1
```

## Mount Partition
```
# mount /dev/sda1 /mnt
```

## Make Directories
```
# mkdir /mnt/{boot,home}
```

## Config Wired Network
_(This is just example, use your static ips accordingly.)_
* Network Adapter: `enp3s0`
* IP Address: `192.168.0.110`
* Broadcast IP: `192.168.0.255`
* Route IP: `192.168.0.1`
```
# ip addr add 192.168.0.110/24 brd 192.168.0.255 dev enp3s0
# ip route add default via 192.168.0.1 dev enp3s0
# ping -c1 archlinux.org   (check to see if it worked)
```

## Install Arch Base System
```
# pacstrap -K /mnt base linux linux-firmware intel-ucode grub nano
```

## Fix Missing "libmpc" Package Required By "base-devel"
_("libmpc" install from archive was required on 2022-12-21, possibly fixed now)_
```
# pacman -U https://archive.archlinux32.org/packages/l/libmpc/libmpc-1.2.1-2.0-pentium4.pkg.tar.zst
# pacman -S base-devel
```

## Config File System
_(Its recommended that you always use `-U` UUIDs instead of `-L` labels.)_
```
# genfstab -U /mnt >> /mnt/etc/fstab
```

## Enter ChRoot
```
# arch-chroot /mnt
```

## Config TimeZone
_(you can find all available zones at `/usr/share/zoneinfo/`)_
```
# ln -sf /usr/share/zoneinfo/YourCountry/YourState /etc/localtime
```

## Sync System Clock
```
# hwclock --systohc
```

## Config Language
_(you can find all available languages at `/etc/locale.gen`)_
```
# mv /etc/locale.gen{,.orig}   (make a backup of original)
# echo "en_US.UTF-8 UTF-8" > /etc/locale.gen   (your language and encoding here)
# locale-gen
# locale > /etc/locale.conf
```

## Config HostName
```
# echo "YourHostName" > /etc/hostname
```

## Config Kernel
```
# mkinitcpio -P
```

## Set Root Password
```
passwd   (and type it twice)
```

## Create Regular User
```
# useradd -m -g users yourusername
# passwd yourusername
```

## Exit ChRoot
Press `CTRL+D` or type `exit` and press `Enter`.

## UnMount Partitions
```
# umount -R /mnt
```

## Reboot System
Type `reboot` and press `Enter` than unplugin USB drive.

** Post Installation **

To be continued...
