# privesc checklist
## Basic privesc checks
* whoami, groups, id
* id user1 (ha a usernev is ott van akkor minden group latszik, amugy csak a main group amibe tartozik)
* sudo -l, crontab -l, ls -la /etc/&ast;cron&ast;, ps aux
* ls /, /home, /root, ./.ssh, /tmp
* cat /etc/passwd, cat /etc/shadow
* netstat -ano
  * netstat -ano | grep "127" (csak belulrol elerheto portok)
  * ss -ano (ha nincs netstat a gepen)
* belulrol elerheto portok
  * curl-el vagy wget-el nezzuk meg belulrol hogy mi van vele, vagy nc-vel ra tudunk csatlakozni
  * vagy portforwarddal kirakjuk az attack gepre (ssh, socat, chisel, meterpreter...)
* /var/www konyvtarat nezzuk at rendesen manualisan
* ha tobb webserver van, akkor szet kell nezni tobb helyen, mert nem minden a /var/www mappaban van!
* GTFObins
* Group membership: docker, lxd, adm, sudo,
* pspy64
* linpeas, LinEnum
* linux exploit suggester scriptek
## Other privesc checks
* SUID files
* capabilities
* readable files and folders
* writeable files and folders
* executable files and folders
* relative file path of binaries, libs and script files
## Kernel exploits, OS vulns, OS util vulns, etc...
* PwnKit, sudo serulekenysegek, stb...
* 
