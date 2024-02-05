# SMB
## null session
smbclient -N -L \\\\192.251.169.3  
smbclient \\\\192.251.169.3\\public  
rpcclient 192.251.169.3 -U'' -N  
* lehet hogy ezt forditva kell
* $ rpcclient -U '' -N 10.10.10.3
## login bruteforcing
### metasploit
auxiliary/scanner/smb/smb_login  (rhosts, smbuser|user_file, smbpass|pass_file)
### hydra
hydra -l admin -p Pass123 smb://192.38.1.3  
hydra -L user_list.txt -P pass_list.txt smb://192.38.1.3  
### 

## Nmap SMB scripts
  nmap -Pn -v 10.2.29.65 -sT -p445 --script=smb-os-discovery  
  nmap -Pn -v 10.2.29.65 -sT -p445 --script=smb-protocols  
  nmap -Pn -v 10.2.29.65 -sT -p445 --script=smb-security-mode  
  nmap -Pn -v 10.2.29.65 -sT -p445 --script=smb2-security-mode  
  nmap -Pn -v 10.2.29.65 -sT -p445 --script=smb2-capabilities  
  nmap -Pn -v 10.2.29.65 -sT -p445 --script=smb-enum-sessions --script-args=smbusername=administrator,smbpassword=Pass123  
  nmap -Pn -v 10.2.29.65 -sT -p445 --script=smb-enum-shares --script-args=smbusername=administrator,smbpassword=Pass123  
  nmap -Pn -v 10.2.29.65 -sT -p445 --script=smb-enum-users --script-args=smbusername=administrator,smbpassword=Pass123  
  nmap -Pn -v 10.2.29.65 -sT -p445 --script=smb-enum-domains --script-args=smbusername=administrator,smbpassword=Pass123  
  nmap -Pn -v 10.2.29.65 -sT -p445 --script=smb-enum-services --script-args=smbusername=administrator,smbpassword=Pass123  
  nmap -Pn -v 10.2.29.65 -sT -p445 --script=smb-enum-groups --script-args=smbusername=administrator,smbpassword=Pass123  
## win cmd net use (list remote shares, connect to remote share)
  net use  
  net use x: \\10.2.30.64\c$ Pass123 /user:administrator  
## win cmd net share (list local shares, create new local share)
  net share  
  net share share_name=C:\path  
  net share Docs=C:\Documents /grant:everyone,FULL  
  net share Docs=E:\Documents /grant:username,READ  
  net share docs /delete  
  net share E:\Docs /delete  
## smbmap
<pre>
smbmap -u guest -p '' -H 10.2.18.153					//list shares with guest user  
smbmap -u administrator -p smbserver_771 -H 10.2.18.153                         //list shares with user and pass  
smbmap -u administrator -p smbserver_771 -H 10.2.18.153 -d .                    //list shares with domain  
smbmap -u administrator -p smbserver_771 -H 10.2.18.153 -x whoami               //run command  
smbmap -u administrator -p smbserver_771 -H 10.2.18.153 -L                      //list drives  
smbmap -u administrator -p smbserver_771 -H 10.2.18.153 -r 'D$'                 //dir share content  
smbmap -u administrator -p smbserver_771 -H 10.2.18.153 -r 'D$\Data'            //dir folder content on share  
smbmap -u administrator -p smbserver_771 -H 10.2.18.153 -r 'D$\Data' --dir-only //dir folder content on share (no files)  
smbmap -u administrator -p smbserver_771 -H 10.2.18.153 -R 'D$\Data'            //recursively dir folder content on share  
smbmap -u administrator -p smbserver_771 -H 10.2.18.153 -R 'D$\Data' --depth 3  //recursively dir folder content on share with given depth  
smbmap -u administrator -p smbserver_771 -H 10.2.18.153 --download 'D$\Data\test.txt'  
smbmap -u administrator -p smbserver_771 -H 10.2.18.153 --upload 'test.txt' 'C$\Program Files\test.txt'  
smbmap -u administrator -p smbserver_771 -H 10.2.18.153 --delete 'C$\Program Files\test.txt'  
</pre>
## nmblookup
nmblookup -A 192.251.169.3  //by ip  
## smbclient
smbclient -L \172.16.5.35 -U peter  
smbclient -N -L \172.16.5.35 -U peter  
smbclient -L \172.16.5.35 -U peter%password  
smbclient \\172.16.5.35\Users -U peter%password  
smbclient -U jason \\10.129.203.6\GGJ  
## rpcclient
srvinfo  
netshareenum  
netshareenumall  
netsharegetinfo share_name  
enumdomusers  
enumdomgroups  
enumdomains  
queryuser  
querygroup  
lookupnames  
lookupsids  
## metasploit modules
auxiliary/scanner/smb/smb_version  
auxiliary/scanner/smb/smb2  
auxiliary/scanner/smb/smb_enumusers  
auxiliary/scanner/smb/smb_enumshares  
auxiliary/scanner/smb/smb_login  
auxiliary/scanner/smb/pipe_auditor  
## enum4linux
enum4linux 192.251.169.3  
enum4linux -U 192.251.169.3  
enum4linux -M 192.251.169.3  
enum4linux -S 192.251.169.3  
enum4linux -G 192.251.169.3  
enum4linux -r 192.251.169.3  (rid cycling)  
enum4linux -R 600-660 192.251.169.3  (rid cycling with custom range)  
### with authentication (IMPORTANT!!!! every switch has to be before the ip address!!!!)
enum4linux -u "administrator" -p "Pass123" 192.251.169.3  
like enum4linux -r -u "administrator" -p "Pass123" 192.251.169.3  
### error handling
[+] Can't determine if host is part of domain or part of a workgroup  
put the switch before the ip like just after the 'enum4linux' command str  
like enum4linux -r -u "administrator" -p "Pass123" 192.251.169.3  
## eternal blue
SMBv1 protocol erintett
see at here: https://github.com/VajdaGergely/pentesting_cheatsheet/blob/main/eternal%20blue.md

