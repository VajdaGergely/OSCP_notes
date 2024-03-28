# Basic Windows Privilege Escalation Checklist
## Situational Awareness
* Hostname, username, current user - privileges and group memberships
  * hostname
  * whoami
  * whoami /priv
  * whoami /groups
* Existing users and groups
  * net user
  * net user User1
  * net localgroup
  * net localgroup Administrators
  * query user
* Operating system, version and architecture
  * systeminfo
* Network information
  * ipconfig /all
  * netstat -ano
  * route print
  * arp -a
* Running processes
  * tasklist
  * tasklist /svc
* Installed applications
  * wmic [ENTER] product get name, version [ENTER]
    * varni kell egy kicsit, nem ad egybol eredmenyt
    * lehet erdemes redirectelni az eredmenyt egy file-ba (product get name, version > output.txt)
