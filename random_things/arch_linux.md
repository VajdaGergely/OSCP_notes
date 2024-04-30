# Sources
* https://archlinux.org/download/
* https://wiki.archlinux.org/title/Installation_guide
* https://phoenixnap.com/kb/swap-partition
* https://unix.stackexchange.com/questions/120221/gpt-or-mbr-how-do-i-know
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
