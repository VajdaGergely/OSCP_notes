# Read bash variable into python oneliner
<pre>
$ export url=1234
$ python3 -c 'import os; url=os.environ["url"]; print(url)'
1234
</pre>
