# Get the argument of the last command and use it
**Ha tobb az argument, akkor az utolsot valasztja ki!!!**
<pre>
user$ echo 'abcdefgh' > file1
user$ ls -l ./file1
-rw-rw-r--  1 user user    9 Feb  9 18:47 file1
user$ <b>cat !$</b>
<b>cat file1</b>
abcdefgh
</pre>
**Ha nem az utolsot szeretnenk, akkor az alabbi segithet (a switch-eket is szamolja!!!)**
<pre>
user$ echo '11111111' > file1
user$ echo '22222222' > file2
user$ echo '33333333' > file3
user$ ls file1 file2 file3
file1 file2 file3
user$ <b>cat !:1</b>
<b>cat file1</b>
11111111
user$ ls -l file1 file2 file3
-rw-rw-r-- 1 user user 9 Feb  9 18:49 file1
-rw-rw-r-- 1 user user 9 Feb  9 18:50 file2
-rw-rw-r-- 1 user user 9 Feb  9 18:50 file3
user$ <b>cat !:1</b>
<b>cat -la</b>
cat: invalid option -- 'l'
Try 'cat --help' for more information.
</pre>
