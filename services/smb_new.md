# SMB enumeration and exploitation steps
* nmap -Pn 10.10.10.3 -p137,138,139,445 -sT -sV -sC
* smbclient -N -L \\10.10.10.3
* smbclient \\\\10.10.10.3\\share1
## OS specific enum and attacks
* sudo nmap -Pn 10.10.10.3 -sT -O
### Linux SMB (Samba)
* enum4linux 10.10.10.3
### Windows SMB
* rpcclient -N -U "" 10.10.10.3
