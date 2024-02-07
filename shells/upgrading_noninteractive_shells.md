# Upgrading noninteractive shells
## simple shells
* cat /etc/shells
* /bin/bash -i
* /bin/sh (bash jobb)
## python shell
* which python vagy python --version
* python -c 'import pty;pty.spwan("/bin/bash")'
## perl shell
* perl --help
* perl -e 'exec "/bin/bash";'
## ruby shell
* ruby: exec "/bin/bash"
## fully interactive tty
* $ (CTRL + Z)
* $ echo $TERM (megmutatja a terminal tipus)
* $ stty -a (megmutatja a rows es columns ertekeket)
* $ stty raw -echo;fg
* $ (press ENTER)
* $ stty rows XX columns YY
* $ export TERM=screen
