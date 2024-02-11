# (Kidolgozas alatt!!!! EJPT tananyagbol es mas helyekrol ki kell rendesen boviteni)
# SMB enumeration and exploitation steps
## Basic enumeration
* nmap -Pn 10.10.10.3 -p137,138,139,445 -sT -sV -sC
* smbclient -N -L \\10.10.10.3 (Null session)
* smbclient \\\\10.10.10.3\\share1
## Searching exploits for specific SMB versions
* Get proper version of smb (nmap -sV nem mindig ad tokeletesen pontos vegeredmenyt)
  * nmap -Pn 10.10.10.3 -sT --script=smb-os-discovery
## Null session
* Amikor usernev nelkul be tudunk jelentkezni es latunk tartalmat, illetve rpc-n keresztul is tudunk interaktalni
### Check null session
* smbclient -N -L \\10.10.10.3
### Interact through rpc
* Windows
  * rpcclient -N -U "" 10.10.10.3
* Linux
  * enum4linux 10.10.10.3
## Nmap scripts

## Password cracking
* Keressunk usert hozza, anelkul nem sok ertelme van elkezdeni.
  * hydra -l user1 -P ./rockyou.txt smb://10.10.10.3
* Ha megis user nelkul akarjuk csinalni akkor celszeru admin userrel tolni (amirol ugye tudjuk hogy mindig ott van)
  * Windows SMB -> 'Administrator'
    * hydra -l Administrator -p ./rockyou.txt smb://10.10.10.3
  * Linux SMB (Samba) -> 'root'
    * hydra -l root -p ./rockyou.txt smb://10.10.10.3
* Ha jo tippunk van a usernevre, vagy tudunk 10-20at ami nagyon igeretes, (vagy ismerjuk az osszes usernevet valahonnan), akkor toljuk user es password listaval
  * hydra -L user_list.txt -P ./rockyou.txt smb://10.10.10.3
## OS specific enum and attacks
* sudo nmap -Pn 10.10.10.3 -sT -O
### Eternal Blue (MS-17-010) (CVE-2017-0144) (Csak Windows!)
* SMBv1-nek engedelyezve kell lennie hozza
* Windows OS serulekeny, Linux nem, az alabbi OS verziok erintettek
  * Windows Vista, 7, 8.1, 10 Windows Server 2008, Server 2012, Server 2016
#### SMBv1 ellenorzese (engedelyezve van e)
* nmap -Pn 10.10.10.3 -sT --script=smb-protocols
#### EternalBlue vulnerability test
* nmap -Pn 10.10.10.3 -sT --script=smb-vuln-ms17-010
#### Exploitation (ha serulekeny)
* msf module
* exploitdb code
