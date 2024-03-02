# read_bash_variable_into_python_oneliner.md
<pre>
$ export url=1234
$ python3 -c 'import os; url=os.environ["url"]; print(url)'
</pre>
