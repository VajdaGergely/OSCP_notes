# SMB enumeration and exploitation steps
## Basic enumeration
* nmap -Pn 10.10.10.3 -p137,138,139,445 -sT -sV -sC
* smbclient -N -L \\10.10.10.3 (Null session)
* smbclient \\\\10.10.10.3\\share1
## Searching exploits for specific SMB versions
* Get proper version of smb (nmap -sV nem mindig ad tokeletesen pontos vegeredmenyt)
  * nmap -Pn 10.10.10.3 -sT --script=smb-os-discovery
## Nmap scripts

## Password cracking

## OS specific enum and attacks
* sudo nmap -Pn 10.10.10.3 -sT -O
### Linux SMB (Samba)
* enum4linux 10.10.10.3
### Windows SMB
* rpcclient -N -U "" 10.10.10.3
* nmap -Pn 10.10.10.3 -sT --script=smb-protocols
  * ha SMBv1 engedelyezett, akkor EternalBlue-t nezzuk meg (EternalBlue, MS-17-010, 
    * csak Windowson van EternalBlue, Linuxon nincs!
    * Windows Vista, 7, 8.1, 10 Windows Server 2008, Server 2012, Server 2016 (ezek a Win OS-ek erintettek)
    * nmap -Pn 10.10.10.3 -sT --script=smb-vuln-ms17-010
      * ha serulekeny, akkor mehet az eploit - msf valtozat vagy manualis searchsploit-os
      * 
