# Python http requests example
<pre>
import requests, sys
data = '-----------------------------146699361832369050221550630126\r\nContent-Disposition: form-data; name="bookurl"\r\n\r\nhttp://127.0.0.1:5000/static/css/bootstrap.min.css\r\n-----------------------------146699361832369050221550630126\r\nContent-Disposition: form-data; name="bookfile"; filename=""\r\nContent-Type: application/octet-stream\r\n\r\n\r\n-----------------------------146699361832369050221550630126--\r\n'
headers = {
	'Host':'editorial.htb',
	'User-Agent':'Mozilla/5.0 (X11; Linux x86_64; rv:127.0) Gecko/20100101 Firefox/127.0',
	'Accept':'*/*',
	'Accept-Language':'en-US,en;q=0.5',
	'Accept-Encoding':'gzip, deflate, br',
	'Content-Type':'multipart/form-data; boundary=---------------------------146699361832369050221550630126',
	'Origin':'http://editorial.htb',
	'Connection':'keep-alive',
	'Referer':'http://editorial.htb/upload',
	'Priority':'u=1',
	'Pragma':'no-cache',
	'Cache-Control':'no-cache'
}

proxies = {
	'http':'http://127.0.0.1:8080'
}

url = 'http://editorial.htb/upload-cover'

result = requests.post(url,data=data,headers=headers,proxies=proxies)

print(result.text)
</pre>
