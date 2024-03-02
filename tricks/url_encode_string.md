# URL encode string
<pre>
<b># url encode python string</b>
$ python3 -c 'import urllib.parse; print(urllib.parse.quote("http://abc.com?a=1&b=2"))'
http%3A//abc.com%3Fa%3D1%26b%3D2

<b># url encode bash string</b>
$ export url=1234
$ python3 -c 'import urllib.parse; import os; url=os.environ["url"]; print(urllib.parse.quote(url))'
</pre>
