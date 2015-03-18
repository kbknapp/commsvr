# Install ALARM image to SDCard

* Zero the beginning of the SD card or eMMC module:

```sh
# dd if=/dev/zero of=/dev/sdX bs=1M count=8
```

* Start fdisk to partition the SD card:

```sh
# fdisk /dev/sdX
o
n
p
1
<ent>
<ent>
w
```
* Create and mount the ext4 filesystem:

```sh
# mkfs.ext4 /dev/sdX1
# mkdir root
# mount /dev/sdX1 root
```

* Download and extract the root filesystem (as root, not via sudo):

```sh
# wget http://archlinuxarm.org/os/ArchLinuxARM-odroid-c1-latest.tar.gz
# bsdtar -xpf ArchLinuxARM-odroid-c1-latest.tar.gz -C root
```

* Flash the bootloader files:

```sh
cd root/boot
./sd_fusing.sh /dev/sdX
cd ../..
```

* Unmount the partition:

```sh
# umount root
```

* Insert the micro SD card or eMMC module into the C1, connect ethernet, and apply 5V power.

# Update to base install

* Use the serial console (with a null-modem adapter if needed) or SSH to the IP address given to the board by your router.
* The default root password is 'root'.
* Update the system

```sh
# pacman -Syu
# lsblk
# mkswap /dev/mmcblk0p2
# swapon /dev/mmcblk0p2
# vi /etc/locale.gen
# locale-gen
# echo LANG=en_US.UTF-8 > /etc/locale.conf
# export LANG=en_US.UTF-8
# rm /etc/localtime
# ln -s /usr/share/zoneinfo/America/New_York /etc/localtime
# echo p01 > /etc/hostname
# passwd
# cd /etc/pacman.d/
# mv mirrorlist mirrorlist.backup
# cp mirrorlist.backup mirrorlist
# vi mirrorlist
# mv mirrorlist mirrorlist-us
# rankmirrors mirrorlist-us > mirrorlist
# pacman -Syy
# pacman -Syu
```
