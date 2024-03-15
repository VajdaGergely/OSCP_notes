# Linux initial foothold checklist
* Get RCE through a public exploit
* Get shell through a public exploit
* File upload capability to web server or a local file system share like FTP, SMB, ...
  * Upload webshell and get RCE through it
    * Use web application's built in file upload function to upload webshell if it is presented and open uploaded file through web application
    * Use FTP to upload webshell in web folder if it is can be done, and open uploaded file through web application
    * Instead of webshell you can upload a reverse shell coded in the appropriate web language and skip RCE and get terminal access instantly
  * Upload self-made SSH keys and gain SSH access to a user
    * Overwriting existing SSH key can be done too. But it can be dangerous
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
  * Method 2
    * Try to read web server source code
    * Try to exploit flaws to get RCE
    * Make reverse shell access through RCE
# Potential further steps
* Privilege escalation to root or another user
* Pivoting to another subnet (if the machine is dual-homed)
* Pillaging for other credentials
* Get domain user credentials!
  * It is needed to do AD enumeration
