# Web enumeration and attacks
## Gobuster
## Ffuf
* ffuf -c -w wordlist.txt -u http://10.10.10.1/FUZZ
## Wfuzz
### get req
* wfuzz -c -z file,/opt/useful/SecLists/Usernames/top-usernames-shortlist.txt --hs "Invalid username" http://94.237.49.11:43661/question1/?Username=FUZZ&Password=dummyPass  
### post req
* wfuzz -c -z file,/opt/useful/SecLists/Usernames/top-usernames-shortlist.txt -d "Username=FUZZ&Password=dummyPass" --hs "Invalid username" http://94.237.49.11:43661/question2/
### time throttling (10 sec)
* wfuzz -c -z file,/opt/useful/SecLists/Usernames/top-usernames-shortlist.txt -d "Username=FUZZ&Password=dummyPass" -s 10 --hs "Invalid username" http://94.237.49.11:43661/question2/
## Hydra http bruteforcing
## Burp tricks
## Etc
