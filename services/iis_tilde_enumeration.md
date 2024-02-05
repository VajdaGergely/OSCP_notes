# IIS tilde enumeration
## info
IIS tilde directory enumeration is a technique utilised to uncover hidden files, directories, and short file names (aka the 8.3 format) on some versions of Microsoft Internet Information Services (IIS) web servers. This method takes advantage of a specific vulnerability in IIS, resulting from how it manages short file names within its directories.  

When a file or folder is created on an IIS server, Windows generates a short file name in the 8.3 format, consisting of eight characters for the file name, a period, and three characters for the extension. Intriguingly, these short file names can grant access to their corresponding files and folders, even if they were meant to be hidden or inaccessible.  

The tilde (~) character, followed by a sequence number, signifies a short file name in a URL. Hence, if someone determines a file or folder's short file name, they can exploit the tilde character and the short file name in the URL to access sensitive data or hidden resources.  

IIS tilde directory enumeration primarily involves sending HTTP requests to the server with distinct character combinations in the URL to identify valid short file names. Once a valid short file name is detected, this information can be utilised to access the relevant resource or further enumerate the directory structure.  

## IIS tilde enumeration manually
The enumeration process starts by sending requests with various characters following the tilde, and bruteforcing the chars one by one:
<pre>
dir name: secre~1
  
http://example.com/~a
http://example.com/~b
http://example.com/~c
...
http://example.com/~s

got HTTP 200 response

http://example.com/~sa
http://example.com/~sb
...
http://example.com/~se

got HTTP 200 response

http://example.com/~sea
http://example.com/~seb
http://example.com/~sec

got HTTP 200 response
...

http://example.com/secre~1/random_file1

</pre>
## IIS-ShortName-Scanner
https://github.com/irsdl/IIS-ShortName-Scanner  
java -jar iis_shortname_scanner.jar 0 5 http://10.129.204.231/  
(Do you want to use proxy [Y=Yes, Anything Else=No]? No)  
