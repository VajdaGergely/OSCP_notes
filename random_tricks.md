# Random tricks
## Get the argument of the last command and use it
**Ha tobb az argument, akkor az utolsot valasztja ki!!!**
<pre>
user$ echo 'abcdefgh' > file1
user$ ls -l ./file1
-rw-rw-r--  1 user user    9 Feb  9 18:47 file1
user$ cat <b>!$</b>
cat file1
abcdefgh
</pre>
**Ha nem az utolsot szeretnenk, akkor az alabbi segithet (a switch-eket is szamolja!!!)**
<pre>
user$ echo '11111111' > file1
user$ echo '22222222' > file2
user$ echo '33333333' > file3
user$ ls -l file1 file2 file3
-rw-rw-r--  1 user user    9 Feb  9 18:47 file1
-rw-rw-r--  1 user user    9 Feb  9 18:47 file2
-rw-rw-r--  1 user user    9 Feb  9 18:47 file3
user$ cat <b>!:2</b>
cat file1
11111111
</pre>
