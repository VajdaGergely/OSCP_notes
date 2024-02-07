# Basic file transfer methods
## Base64 encode - decode - copy/paste between machines
* Base64 encoding data on source machine
* Copy-pasting base64 string from source machine to dest machine
* Base64 decoding on destination machine (+ redirect output to file)
## HTTP file transfer
### From Attack host to Victim host (Linux)
* python web server + wget
### From Attack host to Victim host (Windows)
* python web server + powershell iwr
* python web server + certutil
### From Victim host (Linux) to Attack host
* python web server + wget
### From Victim host (Windows) to Attack host
????
### Alternatives for tools
* alternatives for python: python3, python2.7, php, perl, ruby, ...
* alternatives for wget: curl, powershell iwr, certutil
## SMB file transfer
## SCP file transfer
## Meterpreter file transfer
## Other solutions
* RDP mounted drive
* HTTP file upload server hosted on attack machine + power shell client script
