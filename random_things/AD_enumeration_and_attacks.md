# AD Enumeration and Attacks
## Basic infos
**Goals**  
* learning automated enumeration tools
* learning manual enumeration techniques
* understading why and how the exact flaws and misconfigurations are occured
* attacking form Linux and Windows machine
* using "living off the land" techniques

**Random infos**  
* with NT SYSTEM / AUTHORITY priv on a machine you can enumerate AD without valid credentials

**Tools**
* https://academy.hackthebox.com/module/143/section/1517

**Initial Foothold Methods**
* To establish a foothold in the domain we can obtaining clear text credentials or an NTLM password hash for a user, a SYSTEM shell on a domain-joined host, or a shell in the context of a domain user account.
* Obtaining a valid user with credentials is critical in the early stages of an internal penetration test. This access (even at the lowest level) opens up many opportunities to perform enumeration and even attacks.
## Initial Enumeration of the Domain
* Identifying Hosts (passive enum)
  * Wireshark, tcpdump, pktmon.exe (native on Win10)
    * ARP, MDNS, other tipical layer2 traffic
  * Responder
    * LLMNR, NBT-NS, MDNS
* Identifying Hosts (active enum)
  * fping (scriptable ping command) (faster than ping because it sends ICMP packets to more hosts at the same time)
    * $ fping -asgq 172.16.5.0/24
* Service enumeration
  * nmap
    * $ sudo nmap -v -A -iL hosts.txt -oA ./nmap_output
* Identifying Users
  * Kerbrute
    * https://github.com/ropnop/kerbrute
    * $ kerbrute userenum -d INLANEFREIGHT.LOCAL --dc 172.16.5.5 jsmith.txt -o valid_ad_users
* The local system account NT AUTHORITY\SYSTEM is a built-in account in Windows operating systems. It has the highest level of access in the OS and is used to run most Windows services. It is also very common for third-party services to run in the context of this account by default. A SYSTEM account on a domain-joined host will be able to enumerate Active Directory by impersonating the computer account, which is essentially just another kind of user account. Having SYSTEM-level access within a domain environment is nearly equivalent to having a domain user account.
* There are several ways to gain SYSTEM-level access on a host, including but not limited to:
  * Remote Windows exploits such as MS08-067, EternalBlue, or BlueKeep.
  * Abusing a service running in the context of the SYSTEM account, or abusing the service account SeImpersonate privileges using Juicy Potato. This type of attack is possible on older Windows OS' but not always possible with Windows Server 2019.
  * Local privilege escalation flaws in Windows operating systems such as the Windows 10 Task Scheduler 0-day.
  * Gaining admin access on a domain-joined host with a local account and using Psexec to launch a SYSTEM cmd window
* By gaining SYSTEM-level access on a domain-joined host, you will be able to perform actions such as, but not limited to:
  * Enumerate the domain using built-in tools or offensive tools such as BloodHound and PowerView.
  * Perform Kerberoasting / ASREPRoasting attacks within the same domain.
  * Run tools such as Inveigh to gather Net-NTLMv2 hashes or perform SMB relay attacks.
  * Perform token impersonation to hijack a privileged domain user account.
  * Carry out ACL attacks.
