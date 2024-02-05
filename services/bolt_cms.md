# Bolt CMS
## info
* Bolt is a free, open-source content management system based on PHP. It was released in 2012[3] and developed by Two Kings and the Bolt community. Bolt uses Twig for templates and includes features for content and user management. Bolt can be installed on any Apache or Nginx web server with SQLite, MySQL or MariaDB and PHP 7.2.9 or later.
* default port: 8000/tcp
## RCE (admin priv needed)
* https://book.hacktricks.xyz/network-services-pentesting/pentesting-web/bolt-cms
* get username and password through info leak, or brute force username or password through login page
* login page: http://10.10.10.3/bolt/login
* after login
  * version is at the left bottom section (if you've logged in)
* msf6> use exploit/unix/webapp/bolt_authenticated_rce (3.7.0 folott??)
