# HTTP
## nmap scripts
nmap -Pn -v 10.2.17.173 -p80 -sT -sV -sC  
  
nmap -Pn -v 10.2.17.173 -p80 --script=http-enum  
nmap -Pn -v 10.2.17.173 -p80 --script=http-headers  
nmap -Pn -v 10.2.17.173 -p80 --script=http-methods  
nmap -Pn -v 10.2.17.173 -p80 --script=http-server-header  
nmap -Pn -v 10.2.17.173 -p80 --script=http-title  
nmap -Pn -v 10.2.17.173 -p80 --script=http-robots.txt  
nmap -Pn -v 10.2.17.173 -p80 --script=http-vhosts  
nmap -Pn -v 10.2.17.173 -p80 --script=http-waf-detect  
nmap -Pn -v 10.2.17.173 -p80 --script=membase-http-info  
nmap -Pn -v 10.2.17.173 -p80 --script=http-webdav-scan  
## metasploit modules
auxiliary/scanner/http/title  
auxiliary/scanner/http/http_header  
auxiliary/scanner/http/http_login  
auxiliary/scanner/http/options  
auxiliary/scanner/http/robots_txt  
auxiliary/scanner/http/cert  
auxiliary/scanner/http/ssl  
auxiliary/scanner/http/ssl_version  
auxiliary/scanner/http/http_version  
auxiliary/scanner/http/vhost_scanner  
auxiliary/scanner/http/webdav_scanner  
