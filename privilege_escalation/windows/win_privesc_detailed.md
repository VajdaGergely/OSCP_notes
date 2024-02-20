# Win privesc techniques
## Juicy Potato
* Needed scripts from github (all of them need to be uploaded to the victim machine)
  * JuicyPotato binaries
    * https://github.com/ohpe/juicy-potato
  * ncat binaries
    * https://github.com/andrew-d/static-binaries/blob/master/binaries/windows/x86/ncat.exe
  * Enable privileges
    * https://www.powershellgallery.com/packages/PoshPrivilege/0.3.0.0/Content/Scripts%5CEnable-Privilege.ps1
    * https://www.leeholmes.com/adjusting-token-privileges-in-powershell/
<pre>
C:\> Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass -Force
C:\> Import-Module .\EnableAllTokenPrivs.ps1
C:\> Enable-Privilege -Privilege SeAssignPrimaryTokenPrivilege
C:\> whoami /priv (most mar elvileg Enabled lett)
</pre>
