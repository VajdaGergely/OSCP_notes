# Linux initial foothold checklist
* Get RCE through a public exploit
* Get shell through a public exploit
* Upload webshell and get RCE through it
* Look for usernames and try to reuse them somewhere else
* Access sensitive files that leak credentials, hashes, ssh keys or other secrets
  * that can be used to login through SSH, RDP, ...
  * that can be cracked and then can be used to login through SSH, RDP, ...
* Bruteforcing credentials for SSH, RDP, ...
  * or bruteforcing credentials for other services and try to reuse these credentials to SSH, RDP, ...
* File read access to part or full of the filesystem
  * Method 1
    * Get usernames through cat /etc/passwd
    * Try to read ssh keys with observed usernames like: cat /home/user1/.ssh/id_rsa
    * Use SSH keys to login and crack ssh passphrases if needed
    * Try to reuse SSH passphrase as user password
