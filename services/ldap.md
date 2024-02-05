# ldap
## info
LDAP (Lightweight Directory Access Protocol) is a protocol used to access and manage directory information.  
Ports: tcp/389, tcp/636  
## ldapsearch
command-line utility used to search for information stored in a directory using the LDAP protocol  
commonly used to query and retrieve data from an LDAP directory service  
<pre>
(request)
ldapsearch -H ldap://ldap.example.com:389 -D "cn=admin,dc=example,dc=com" -w secret123 -b "ou=people,dc=example,dc=com" "(mail=john.doe@example.com)"  

request info:
* Connect to the server ldap.example.com on port 389.
* Bind (authenticate) as cn=admin,dc=example,dc=com with password secret123.
* Search under the base DN ou=people,dc=example,dc=com.
* Use the filter (mail=john.doe@example.com) to find entries that have this email address.

(response)

dn: uid=jdoe,ou=people,dc=example,dc=com
objectClass: inetOrgPerson
objectClass: organizationalPerson
objectClass: person
objectClass: top
cn: John Doe
sn: Doe
uid: jdoe
mail: john.doe@example.com

result: 0 Success
</pre>
## ldap injection
LDAP injection is an attack that exploits web applications that use LDAP (Lightweight Directory Access Protocol) for authentication or storing user information. The attacker can inject malicious code or characters into LDAP queries to alter the application's behaviour, bypass security measures, and access sensitive data stored in the LDAP directory.  
To test for LDAP injection, you can use input values that contain special characters or operators that can change the query's meaning:  
<pre>
Input	  Description
*	      An asterisk * can match any number of characters.
( )	    Parentheses ( ) can group expressions.
|	      A vertical bar | can perform logical OR.
&	      An ampersand & can perform logical AND.
(cn=*)	Input values that try to bypass authentication or authorisation checks by injecting conditions that always evaluate to true can be used. For example, (cn=*) or (objectClass=*) can be used as input values for a username or password fields.
</pre>
### example
nmap scan reveales that ldap is used on the host  
if ldap is used for authentication and no user input validation in use we can try to login with *:* as ldap wildcard character  
<pre>
$ nmap -p- -sC -sV --open --min-rate=1000 10.129.204.229

Starting Nmap 7.93 ( https://nmap.org ) at 2023-03-23 14:43 SAST
Nmap scan report for 10.129.204.229
Host is up (0.18s latency).
Not shown: 65533 filtered tcp ports (no-response)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT    STATE SERVICE VERSION
80/tcp  open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-server-header: Apache/2.4.41 (Ubuntu)
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
|_http-title: Login
389/tcp open  ldap    OpenLDAP 2.2.X - 2.3.X

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 149.73 seconds
</pre>
