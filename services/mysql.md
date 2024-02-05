# MySQL
## linux mysql client
mysql -u username -p password  
mysql -h hostname -u username -p password  
mysql -h hostname  

## mysql internal commands
show databases;  
use database_name;  
show tables;  
select version();  
select user();  
select database();  
<pre>\! sh (get a shell with mysql user!!!!!)</pre>
select * from mysql.user;
select load_file("/filepath/filename");

## metasploit modules
### !default username root is needed, if no username specified!
auxiliary/scanner/mysql/mysql_version  
auxiliary/scanner/mysql/mysql_schemadump (username is needed in every circumstances, the default is root)  
auxiliary/scanner/mysql/mysql_writable_dirs  
auxiliary/scanner/mysql/mysql_file_enum  
auxiliary/scanner/mysql/mysql_login  
auxiliary/scanner/mysql/mysql_hashdump  

## nmap scripts
nmap 192.168.10.1 --script=mysql-emtpy-password  
nmap 192.168.10.1 --script=mysql-info  
nmap 192.168.10.1 --script=mysql-users  
nmap 192.168.10.1 --script=mysql-variables  
nmap 192.168.10.1 --script=mysql-enum --srcipt-args=mysql-enum.userdb=root,mysql-enum.passdb=Pass123  
nmap 192.168.10.1 --script=mysql-query  
nmap 192.168.10.1 --script=mysql-databases  
nmap 192.168.10.1 --script=mysql-dump-hashes  
nmap 192.168.10.1 --script=mysql-dump-query  
<pre>
nmap -v -Pn 192.186.44.3 --script=mysql-audit --script-args=mysql-audit.username=root,mysql-audit.password='Pass123',mysql-audit.filename='/usr/share/nmap/nselib/data/mysql-cis.audit'
(default audit script is: /usr/share/nmap/nselib/data/mysql-cis.audit) (maybe all argument needed to run it properly and maybe full path of audit script is needed too)
</pre>
