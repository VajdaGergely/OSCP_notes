# Rendes feladat
* sources:
* basic (GUI-t mar ne rakjuk fel!!!!)
  * https://www.youtube.com/watch?v=xMjI8pypeJE&ab_channel=DWIX
* encrypted partitions
  * https://devconnected.com/how-to-encrypt-partition-on-linux/
# Debuggolas
* sima external hard drive-ot auto-mount-olni (pipa)
* luks drive-ot hozzaadni, ami kibontja magat boot-olaskor (pipa)
* luks drive auto-mount
  * error-t kapok
  * valahogy debuggolni kene
  * pl chown-al allitsuk be a sima user-re, a group is legyen az ove, nezzuk is meg, hogy kepes e belemenni
  * adjunk neki jogot hogy irja olvassa vegrehajtsa
# A feladat mashogy nez ki
* vmware kell!!!
* le kell dokumentalni az egeszet!!!
* mas GUI kell
* stb...
# Install KDE GUI
* https://www.youtube.com/watch?v=Csc8Y5_YLRE&ab_channel=DanHenry
# Progress
* OS install GUI-val pipa
# Sources
* https://archlinux.org/download/
* https://wiki.archlinux.org/title/Installation_guide
* https://phoenixnap.com/kb/swap-partition
* https://unix.stackexchange.com/questions/120221/gpt-or-mbr-how-do-i-know
# Install xfce GUI on linux arch
* source: https://www.youtube.com/watch?v=FfGzL9zhPoU&ab_channel=DanHenry
# Install arch linux with UEFI + GPT
* source: https://www.youtube.com/watch?v=xMjI8pypeJE&ab_channel=DWIX
  * GUI-t ne ez alapjan rakjuk fel
  * hanem az alabbi alapjan
    * https://www.youtube.com/watch?v=FfGzL9zhPoU&ab_channel=DanHenry
# Update
* A grub+bios+mbr keszen van
* Csapassuk a grub+uefi+gpt-t
## EFI system partition needed
* UEFI + GPT
  * UEFI systems typically need an EFI system partition.
* BIOS + GPT
  * BIOS systems that are partitioned with GPT require a BIOS boot partition if GRUB is used as the bootloader.
## Boot partition needed (probably the EFI and the boot partition are the same partitions we are talking about)
* A separate physical (in the main partition table of the disk, not under LVM, software RAID or in a file system subvolume etc.) /boot partition is needed unless your boot loader is capable of accessing the /boot directory that resides in /.
## EFI and boot
* When using an EFI system partition as /boot, the requirements are as described in the EFI system partition article—the correct partition type must be set.
# UEFI + GPT layout
**UEFI/GPT layout example**
|Mount point on the installed system|Partition|Partition type GUID|Suggested size|
|---|---|---|---|
|/boot or /efi1|/dev/sda1|C12A7328-F81F-11D2-BA4B-00A0C93EC93B: EFI system partition|1 GiB|
|[SWAP]|/dev/sda2|0657FD6D-A4AB-43C4-84E5-0933C84B4F4F: Linux swap|At least 4 GiB or the size of RAM to use hibernation|
|/|/dev/sda3|4F68BCE3-E8CD-4DB1-96E7-FBCAF984B709: Linux x86-64 root (/)|Remainder of the device. At least 23–32 GiB.|
# Update
* ez alapjan csinaljuk
  * https://www.youtube.com/watch?v=FudOL0-B9Hs&ab_channel=EF-LinuxMadeSimple
<pre>
$ ip a
$ timedatectl set-ntp true
$ lsblk
$ (sda should be presented with type disk)
$ fdisk /dev/sda
  >n
  >[ENTER]
  >[ENTER]
  >+4G
  >n
  >[ENTER]
  >[ENTER]
  >[ENTER]
  >t
  >2
  >82
  >p
  >w
  >q
$ lsblk
$ mkfs.ext4 /dev/sda2
$ mkswap /dev/sda1
$ swapon /dev/sda1
$ mount /dev/sda2 /mnt
$ lsblk
$ pacstrap /mnt base linux nano
$ genfstab -U /mnt >> /mnt/etc/fstab
$ cat /mnt/etc/fstab
$ arch-chroot /mnt
$ ln -sf /usr/share/zoneinfo/Europe/Budapest /etc/localtime
$ hwclock --systohc
$ nano etc/locale.gen
(uncomment en_US.UTF-8 UTF-8)
$ locale-gen
$ echo "LANG=en_US.UTF-8" > /etc/locale.conf
$ echo "arch-test" > /etc/hostname
$ echo "127.0.0.1	localhost" >> /etc/hosts
$ echo "::1	localhost" >> /etc/hosts
$ echo "127.0.1.1	arch-localdomain	arch" >> /etc/hosts
$ passwd
(type password, and type again)
$ pacman -S grub networkmanager  network-manager-applet dialog wireless_tools wpa_supplicant os-prober mtools dosfstools base-devel linux-headers
$ grub-install --target=i386-pc /dev/sda
$ grub-mkconfig -o /boot/grub/grub.cfg
$ exit
$ umount -a
$ reboot
(eject iso installer)
</pre>
# Update (megse kell)
* kell a /boot/efi mappa
* es a partitionalasnal
  * a boot partition megy a sima primary partition-re
  * es utana pedig egy extended partition kell
  * es arra megy majd egy logikai partition es ugy fog csak mukodni szepen nekunk!!! 
<pre>
$ pacman -S grub efibootmgr
$ grub-install --target=x86_64-efi --efi-directory=esp --bootloader-id=GRUB
</pre>
# partitions and boot loader
* mindenhol az egyszerubbet valasztottuk
  * MBR, grub, BIOS
* kesobb megprobalhatjuk a bonyolultabb dolgokat is]
  * GPT (MBR helyett)
  * UEFI (BIOS helyett)
  * grub helyett esetleg valami mas
# chroot
* megvaltoztatjuk a root konyvtarat es onnantol minden amit csinalunk, az az uj root konyvtarra vonatkozik majd
* es a file rendszer kulso resze elerhetetlenne valik
* tipikusan olyankor jo, amikor valami live cuccbol bootolunk be vagy extern cuccbol bootolunk be
  * es egy uj file rendszert csinalunk valami adattarolora, ami majd a vegleges fajlrendszer lesz a napi hasznalatban
  * es akkor a live kornyezetbol atlepunk az uj eszkozre pl /mnt/valami...
  * es akkor mar a fel-mount-olt kornyezetben dolgozunk
## no nano in arch-chroot
* https://bbs.archlinux.org/viewtopic.php?id=156519
