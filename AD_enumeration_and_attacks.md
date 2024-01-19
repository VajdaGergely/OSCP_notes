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
    * $ sudo nmap -v -A -iL hosts.txt -oN ./nmap_output
