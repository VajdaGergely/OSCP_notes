# Random tricks
## Get the argument of the last command and use it
<pre>
user$ echo 'abcdefgh' > file1
user$ ls -l ./file1
-rw-rw-r--  1 user user    8 Feb  9 18:47 file1
user$ cat <b>!$</b>
cat file1
abcdefgh
</pre>
