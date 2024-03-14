# Powershell script samples
## Get raw output (get only the value of choosen result field)
<pre>
PS C:\> Get-FileHash .\test.txt
Algorithm       Hash                                                                   Path
---------       ----                                                                   ----
SHA256          8D969EEF6ECAD3C29A3A629280E686CF0C3F5D5A86AFF3CA12020C923ADC6C92       C:\test.txt

PS C:\> Get-FileHash .\test.txt | ForEach-Object -MemberName Hash
8D969EEF6ECAD3C29A3A629280E686CF0C3F5D5A86AFF3CA12020C923ADC6C92
</pre>
