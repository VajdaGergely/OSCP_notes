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
## File transfers with SMB
* Ha van valami a share-ben mar eleve, akkor azt le tudjuk tolteni
* Ha lehet feltolteni, akkor webshell-t feltolthetunk vagy mas egyebet, ami valahogy lefut magatol a server oldalon, vagy elerem mashonnan pl HTTP-n es en tudom majd futtatni
* Ha initial foothold meg volt, akkor
  * Tool-okat tudok felmasolni, pl SharpHound
  * Vagy ertekes adatokat, tool output-ot tudok letolteni (id_rsa, sharphound result file-ok, stb...)
## Nmap scripts
* ...
* <i>(Csak azokat a scripteket gyujtsuk ide ossze amikkel tudjuk is hogy utana mit akarunk csinalni, mire jo az output, hogy kell ertelmezni azokat...)</i>
## User enumeration
* ha rpcclient-el vagy enum4linux-al (vagy mas egyebbel) userneveket talalunk
  * Windows-ben ezek Windows userek lesznek! Nincs kulon service user az SMB-hez
    * Lehetnek local userek, vagy AD userek is, ha a gep AD-ban kotve (ott valszeg domain/ is lesz a usernev elott)
## Ha valid credential-oket szerzunk
* Barmit enumeraltunk eddig SMB-n az uj userekkel ujra vegig kell menni mindenen -> share-ek, rpc, ...
  * Az uj user hozzaferhet olyasmihez amihez az elozo user nem fert hozza, es uj dolgok is latszodhatnak amik eddig nem latszodtak!
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
* Ha PSExec-hez akarunk bruteforcolni
  * Akkor ott egybol az 'Administrator' account-ot bruteforcoljuk
## OS specific enum and attacks
### Get OS of victim machine
* sudo nmap -Pn 10.10.10.3 -sT -O
* msf module
### Linux Attacks
#### Example Vulnerable Samba Version - Samba 3.0.20
* user_map valami exploit van hozza
* msf module van ra, (lehet hogy sima exploitdb-s is de az nem biztos)
* nem kell cred a futtatashoz
* es root shell-t ad
### Windows Attacks
#### Eternal Blue (MS-17-010) (CVE-2017-0144) (Csak Windows!)
* SMBv1-nek engedelyezve kell lennie hozza
* Windows OS serulekeny, Linux nem, az alabbi OS verziok erintettek
  * Windows Vista, 7, 8.1, 10 Windows Server 2008, Server 2012, Server 2016
##### SMBv1 ellenorzese (engedelyezve van e)
* nmap -Pn 10.10.10.3 -sT --script=smb-protocols
##### EternalBlue vulnerability test
* nmap -Pn 10.10.10.3 -sT --script=smb-vuln-ms17-010
##### Exploitation (ha serulekeny)
* msf module vagy searchsploit exploit
* automatikusan NT SYSTEM / AUTHORITY shell-t kapunk vele
#### AD domain-ben teljesen mas modon is lehet tamadni az SMB-t
* Ha vannak userek, akkor Responderrel megprobalhatunk NTLM hash-t capture-olni, vagy tovabb relay-elni
* A hash-t feltorhetjuk vagy PassTheHash attack-el authentikalunk valahova
* Crackmapexec-el sok mindent lehet csinalni SMB keresztul
* ...
#### PSexec
* Admin jogosultsag kell hozza
  * Azert kell admin, mert service-t kell elinditani (vagy piszkalni valahogy) es azt mezei user joggal nem lehet
* NT SYSTEM / AUTHORITY shell-t kapunk vele
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
## random tovabbi cuccok
<pre>
smbmap
lehet vele kodot is futtatni

elvileg linuxon meg windowson is megy

google> psexec.py vs smbmap
</pre>
<pre>
ha smbclienttel csatlakozunk egy linux gephez akkor ott lehet hogy a \\\\ip helyett //ip modon kell megadnunk a targetet
</pre>
<pre>
google> smbmap command execution on linux??
</pre>
<pre>
enum4linux ki tudja szedni a password policyt ami password sprayinghez bruteforcinghoz nagyon hasznos lehet!
</pre>
<pre>
[nmap scripts]
smb-protocols
milyen smb verziok tamogatottak
-v1 windowson serulekeny eternal blue-ra

smb-security-mode
smb2-security-mode
(netes dokumentacio hasznos a v1-hez ott sok lehetoseg van)
-account used
-message signing disabled (nem tudjuk mire jo)
</pre>
<pre>
smb-enum-sessions
kik vannak bejelentkezve epp
</pre>
<pre>
share-eket ne smbclienttel enumeraljunk kizarolag

nmap smb-enum-shares, smbmap, enum4linux -> sokkal faszabb outputot adnak
</pre>
<pre>
ha semmi sem megy az smb-n akkor vegso soron elkezdhetunk rajta mindenfele enzmetation scripteket tolni! De tenyleg csak a legvegen szarozzunk ilyesmivel

Smbmap, enum4linux, nmap scripts
</pre>
<pre>
szinte minden scriptnel jelentosege van hogy adunk e meg user credet hozza illetve hogy melyikeket
</pre>
<pre>
smb-enum-shares, smb-ls
ha igy futtatjuk a ket nmap scriptet akkor rekurzivan kilistazza nekunk a tartalmat a share-eknek
</pre>
<pre>
null session
-elvileg SMBv1 kell hozza es csak ott mukodik...
</pre>
<pre>
smbmap-al lehet parancsot futtatni--
</pre>
<pre>
nem tolt be egy uzenetet
az volt az uzi hogy smbmap-al lehet parancsot is futtatni...

-----

Ha van null session es az IPC share elerheto akkor mindenkepp menjunk ra az rpcclient-el!!
linuxon is mukodik meg windowson is!!
</pre>
<pre>
Ha null session mukodik vagy van mar egy darab valid userunk
--> akkor az osszes tobbi usert is le tudjuk kerni!!! valamelyik script vagy tool tuti hogy visszaadja!!
nmap, smbmap, enum4linux, ...
</pre>
<pre>
*** tool listahoz -> rpcclient
</pre>
<pre>
ha share-eket enumeralunk
abbol is kiderulhetnek userek!!

most mar ne ugy tekintsunk az smb-re mint egy service a sok kozul!!!
hanem ez egy kozponti resze az OS-eknek es nagyon melyen atszovi az OS fintos reszeit!!!
es ezen keresztul sok informaciot tudunk gyujteni amit utana akar teljesen mas helyeken fogunk majd valamihez felhasznalni!!!!
</pre>
