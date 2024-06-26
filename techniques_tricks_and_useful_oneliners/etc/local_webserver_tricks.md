# Local webserver tricks
## Basic python webserver
<pre>
$ python -m http.server 8080
</pre>
## Python webserver plus cgi scripts
* source: https://realpython.com/python-http-server/
<pre>
$ mkdir cgi-bin
$ chmod +x cgi-bin
$ echo '#!/usr/bin/env python3' >> cgi-bin/random.py
$ echo 'print("abcd")' >> cgi-bin/random.py
$ python3 -m http.server --cgi 8080
</pre>
