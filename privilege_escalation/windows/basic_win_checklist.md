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
* Installed applications
* Running processes
