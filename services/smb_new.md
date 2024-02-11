# SMB enumeration and exploitation steps
## Basic enumeration
* nmap -Pn 10.10.10.3 -p137,138,139,445 -sT -sV -sC
* smbclient -N -L \\10.10.10.3
* smbclient \\\\10.10.10.3\\share1
## Nmap scripts

## Password cracking

## OS specific enum and attacks
* sudo nmap -Pn 10.10.10.3 -sT -O
### Linux SMB (Samba)
* enum4linux 10.10.10.3
### Windows SMB
* rpcclient -N -U "" 10.10.10.3
* ha SMBv1 allowed es az OS Windows (pontosabban: Windows Vista, 7, 8.1, 10 Windows Server 2008, Server 2012, Server 2016)
