# Regex samples
## all rows that contains (http://)
<pre>
http:\/\/.*
</pre>
## all lines that not start with 'abcd'
<pre>
^(?:(?!abcd).)*$
^((?!abcd).)*$
</pre>
## string variations with constant prefix
* matches to the followings:
  * name123 peter
  * name123 steve
  * name123 alice
<pre>
(?:name123 )(peter|steve|alice)
</pre>
