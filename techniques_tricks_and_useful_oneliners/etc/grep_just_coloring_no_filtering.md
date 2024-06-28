# Using grep for coloring only without filter anything
* a ^ karakter a sor eleje, es mivel nem irtunk utana semmit, az osszes sorra match-el, viszont kiszinezni nem lehet mert nem egy rendes karakter
* a tobbi resz a filternel mar nem filterez, mivel OR kapcsolat van koztuk es a mindenre match felulirja oket a filternel
* viszont a filter szoveget be fogja szinezni ahogy akartuk
<pre>
$ cat file1.txt | grep -E --color=always '^|filter|filter2'
</pre>
