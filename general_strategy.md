# General strategy
* Initial foothold on a machine
* Make persistence
* Privilege escalation, Pillaging, Lateral movement
* AD compromise
* (AD persistence)
# Different cases
* Got foothold an a linux machine
  * Make persistence
  * Privesc to root (or another user)
  * Pillage for sensitive data and credentials
  * Pillage for domain user credentials
  * Try to pivot further to other subnets
  * Move laterally to another machine (Windows machines are better)
* Got foothold on a windows machine
  * Make persistence
  * Prives to local admin and then to nt authority / system
* If you are local admin on a windows machine
  * Gather credentials from SAM, lsass, NTDS.dit, ...
  * Try to get domain credentials for AD enum
* If you have domain credentials
  * Start AD enumeration and exploitation
