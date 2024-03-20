Win privesc

Szinten ra kene fekudni!!!!

Oscp konyvben fasza dolgok vannak!!
Nezzuk at
Meg a htb academy win privescet is nezzuk at!!


-----------------------------
Olvassuk ujra az egesz oscp konyv pivoting fejezetet

-----------------------------
Http tunneling dns tunneling
Csak akkor kell ha le van limitalva a forgalom

Http - chisel
Dns - dnscat


-----------------------------
Sshuttle

Vpn-szeru csatornat csinal.
Beallit routing szabalyokat.

Socks nelkul ugy tudunk tavoli 2es 3as subnetekben levo gepekkel kommunikalni mintha azok velunk egy alhalon lennenek.




-----------------------------
Github oscp notesra irjuk fel az egyes lehetosegeket valami ertelmes formaban.

Milyen esetek vannak amit szeretnenk es ahhoz mi kell.
Pl pivot 1 gepen keresztul
Pivot 2 gepen keresztul

Windowsrol linuxrol

Mi van ha portok tiltva vannak vagy egy service/tool nem elerheto

Valami ertelmes rendszerbe kell foglalni az osszes esetet.

-----------------------------
Remote portforward annyira nem lenyeges mert ugyanazt lehet vele csinalni csak a masik oldalrol indul el a kapcsolat felepitese.

De a vegen szinten a kalirol kuldunk parancsokat es a target gepek fogadjak a parancsokat.

-----------------------------
Ujranezzuk meg a htb academy pivotingot es az oscp konyv pivotingot is.

Es fesuljuk ossze a kettot!!!!

-----------------------------
Ssh local portforward
-csinalhatunk olyat hogy az 1. Gepen inditjuk el, a 2. Gephez kapcsolodik de az 1. Gepen listenel egy porton es a 3. Gepre tovabbitja a kereseket.

Igy nem a kalin fut es 3 melysegig erunk el vele gepeket a kalirol ugy hogy az ekso gepnek kuldjuk a csomagokat.

Dynamic port forwarddal pedig a 3. Subneten elerjuk barmelyik gep barmelyik portjat.



-----------------------------
Port forwarding
Ha a target machine-nak localhostos service-e fut egy porton.
Akkor kirakjuk a portot hogy kivulrol is elerheto legyen.

Ha tovabb pivotalunk es a 2. Subnetben van egy host aminek a portjat el akarjuk erni. (Es mar tudjuk hogy van ott nyitott port anelkul hogy eleve tunnelingeztunk volna a network discoveryhez)
Akkor a pivothoston beallithatunk port forwardot - hogy kinyitunk egy portot a pivothoston es azt tovabb pakoljuk a tavoli host nyitott portjara.
