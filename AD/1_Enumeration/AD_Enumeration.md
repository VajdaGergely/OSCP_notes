# AD Enumeration
## Metodology
* Valoszinuleg barhonnan is futtatjuk a parancsokat (lokalban egy victim hostrol vagy remote-ban a kalirol) vegso soron a dc fog valaszolni
  * Ha a victim gepen futtatjuk lokalba, akkor az mivel domain joined, a dc-hez fordul a dc ldap portjan keresztul es onnan keri le az infokat es utana a victim gepen visszajon az output (vagy a terminalba powershell scriptnel vagy kiirodik a fajlrendszerbe a sharphound-nal)
  * Ha a kalirol futtatjuk remote-ba akkor
    * vagy a dc maga a victim gep, es vele kommunikalunk, nyilvan akkor a nyitott ldap porton keresztul (ahova valoszinuleg manualis ldap kereseket is kuldhetnenk egyebkent)
    * vagy a victim gep a cel es valahogy azon keresztul csatornazodik at a forgalom 
## Methods
* From Windows
  * What is needed
    * Valid Domain Credentials
    * Shell, amin keresztul a lokalis fajlrendszerben tudunk dolgozni (tool-ot felmasolni, binaris fajlt vagy powershell fajlt futtatni, powershell parancsokat kiadni, ...)
  * Methods we can use
    * Manual powershell scripting (creating LDAP objects, building ldap queries, handling ldap response, etc...)
    * PowerView functions
    * SharpHound + BloodHound
* From Linux
  * What is needed
    * Valid Domain Credentials
  * Methods we can use
    * Bloodhound.py (sharphound-ot helyettesiti, remote-ban tudjuk a kali-rol futtatni es megcsinalja a scannelest aztan a kali-ra hozza letre az output fajlokat, amik a bloodhound-nak kellenek)
