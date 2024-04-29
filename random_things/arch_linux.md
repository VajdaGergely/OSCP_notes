# Arch Linux
## sources
* https://archlinux.org/download/
* https://wiki.archlinux.org/title/Installation_guide
* https://phoenixnap.com/kb/swap-partition
* https://unix.stackexchange.com/questions/120221/gpt-or-mbr-how-do-i-know
# partitions and boot loader
* mindenhol az egyszerubbet valasztottuk
  * MBR, BIOS
* kesobb megprobalhatjuk a bonyolultabb dolgokat is]
  * GPT (MBR helyett)
  * UEFI (BIOS helyett)
# chroot
* megvaltoztatjuk a root konyvtarat es onnantol minden amit csinalunk, az az uj root konyvtarra vonatkozik majd
* es a file rendszer kulso resze elerhetetlenne valik
* tipikusan olyankor jo, amikor valami live cuccbol bootolunk be vagy extern cuccbol bootolunk be
  * es egy uj file rendszert csinalunk valami adattarolora, ami majd a vegleges fajlrendszer lesz a napi hasznalatban
  * es akkor a live kornyezetbol atlepunk az uj eszkozre pl /mnt/valami...
  * es akkor mar a fel-mount-olt kornyezetben dolgozunk
## no nano in arch-chroot
* https://bbs.archlinux.org/viewtopic.php?id=156519
