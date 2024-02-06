# Local portforwarding withouth SSH
## Local portforwarding with netcat
<pre>
# download socat
attack-host$ wget https://github.com/andrew-d/static-binaries/blob/master/binaries/linux/x86_64/socat

# transfer binary to victim host
...

# set local portforwarding on victim host
victim-host$ ./socat tcp-l:4567,fork,reuseaddr tcp:127.0.0.1:8080

# use tools on attack machine against listening remote port
attack-host$ nmap -Pn -v victim-host.local -p4567
PORT   STATE SERVICE
4567/tcp open  http
</pre>
## socat??
## chisel??
