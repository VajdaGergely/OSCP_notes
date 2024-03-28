# Basic Windows Privilege Escalation Checklist
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
* Network information
  * ipconfig /all
  * netstat -ano
  * route print
  * arp -a
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
## Powershell history
<pre>
PS> Get-History

PS> (Get-PSReadlineOption).HistorySavePath
C:\Users\dave\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadLine\ConsoleHost_history.txt

PS> type C:\Users\dave\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadLine\ConsoleHost_history.txt
</pre>
## Service Binary Hijacking
* PEN200.pdf 500. oldal...
