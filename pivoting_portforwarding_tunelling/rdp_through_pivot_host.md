# RDP connection through pivot host (sketch)
* Pivot hoston keresztul rdp-t is lehet tolni.

* Local portforwarddal

* Attack hoston kell kiadni a parancsot.
* A localport a 127.0.0.1re vonatkozik vagyis az attack hostra.

* Es a remote port meg a vegso dest hostra. Ami 3389 standard rdp lesz.
* Es a pivot hostra csatlakozunk ssh-val.

* ssh -i dmz01_key -L 13389:172.16.8.20:3389 root@10.129.203.111
