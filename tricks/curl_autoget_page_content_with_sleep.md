#
<pre>
$ while true; do echo -n "Response length: "; curl -s http://exfiltrate.htb:35335/log | wc -c; sleep 10s; done
Response length: 776
Response length: 776
Response length: 776
Response length: 784
</pre>
