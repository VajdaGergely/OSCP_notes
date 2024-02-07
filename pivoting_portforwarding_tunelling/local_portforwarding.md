# Local portforwarding
## Local portforwarding with SSH
<pre>
# basic simple
ssh -L 1234:127.0.0.1:8080 user1@10.10.138.76

# with private key
ssh -i .ssh/id_rsa -L 1234:127.0.0.1:8080 user1@10.10.138.76

# no terminal (open in backgrounds)
# &: opens in background
# -N: no prompt
# >/dev/null 2>&1: block output and error
ssh -N -L 1234:127.0.0.1:8080 user1@10.10.138.76 >/dev/null 2>&1 &
</pre>
## Local portforwarding with socat
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
## Local portforwarding with Meterpreter
<pre>
meterpreter> portfwd add -l 7878 -r &lt;victim_ip&gt; -p 8080
</pre>
