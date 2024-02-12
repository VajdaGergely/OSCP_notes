# (Kidolgozas alatt!!!! EJPT tananyagbol, HTBa module-okbol es mas helyekrol ki kell rendesen boviteni)
# SMB enumeration and exploitation steps
## Basic enumeration
* nmap -Pn 10.10.10.3 -p137,138,139,445 -sT -sV -sC
* smbclient -N -L \\10.10.10.3 (Null session)
* smbclient \\\\10.10.10.3\\share1
## Searching exploits for specific SMB versions
* Get proper version of smb (nmap -sV nem mindig ad tokeletesen pontos vegeredmenyt)
  * nmap -Pn 10.10.10.3 -sT --script=smb-os-discovery
* Search for vulnerabilities of specific SMB version
  * msfconsole
  * searchsploit
## Null session
* Amikor usernev nelkul be tudunk jelentkezni es latunk tartalmat, illetve rpc-n keresztul is tudunk interaktalni
### Check null session
* smbclient -N -L \\10.10.10.3
### Interact through rpc
* Windows es Linux (elvileg mindket OS-en megy mind a ketto command)
  * rpcclient -N -U "" 10.10.10.3
  * enum4linux 10.10.10.3
## Nmap scripts
## User enumeration
* ha rpcclient-el vagy enum4linux-al (vagy mas egyebbel) userneveket talalunk
  * Windows-ben ezek Windows userek lesznek! Nincs kulon service user az SMB-hez
    * Lehetnek local userek, vagy AD userek is, ha a gep AD-ban kotve (ott valszeg domain/ is lesz a usernev elott)
## Ha valid credential-oket szerzunk
* Barmit enumeraltunk eddig SMB-n az uj userekkel ujra vegig kell menni mindenen -> share-ek, rpc, ...
  * Az uj user hozzaferhet olyasmihez amihez az elozo user nem fert hozza!!!
##  Bruteforcing SMB credentials
* Keressunk usert hozza, anelkul nem sok ertelme van elkezdeni.
  * hydra -l user1 -P ./rockyou.txt smb://10.10.10.3
* Ha megis user nelkul akarjuk csinalni akkor celszeru admin userrel tolni (amirol ugye tudjuk hogy mindig ott van)
  * Windows SMB -> 'Administrator'
    * hydra -l Administrator -p ./rockyou.txt smb://10.10.10.3
  * Linux SMB (Samba) -> 'root'
    * hydra -l root -p ./rockyou.txt smb://10.10.10.3
* Ha jo tippunk van a usernevre, vagy tudunk 10-20at ami nagyon igeretes, (vagy ismerjuk az osszes usernevet valahonnan), akkor toljuk user es password listaval
  * hydra -L user_list.txt -P ./rockyou.txt smb://10.10.10.3
* Lehet password sparying-et is csinalni, es akkor valami user listan megyunk vegig 1-2 default jelszoval
  * Az segithet ha szerzunk listat a tenyleges userekrol
  * Az is segithet, ha valahol talalunk egy jelszot - user nelkul, es azt hasznaljuk password sprayingra
## AD domain-ben teljesen mas modon is lehet tamadni az SMB-t
* Ha vannak userek, akkor Responderrel megprobalhatunk NTLM hash-t capture-olni, vagy tovabb relay-elni
* A hash-t feltorhetjuk vagy PassTheHash attack-el authentikalunk valahova
* Crackmapexec-el sok mindent lehet csinalni SMB keresztul
* ...
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
* searchsploit
## etc
### message signing
* igy lehet megnezni
  * nmap -Pn 10.10.10.3 -sT --script=smb-security-mode
  * nmap -Pn 10.10.10.3 -sT --script=smb2-security-mode
* az alabbi ertekeket kaphatja
  * enabled
  * enabled but not required
  * disabled
* not required vagy diasbled, akkor lehet sniffingelni (pl Responderrel valszeg)
* allitolag a PSExec hasznalathoz is kell hogy not required vagy disabled legyen (de ez valszeg false info...)
