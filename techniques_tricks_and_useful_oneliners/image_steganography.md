# Image steganography (extraction of hidden information from images)
## scanning images and extracting hidden info (with valid password)
* file image.jpg
* strings image.jpg
* steghide info image.jpg (ha ker passwordot is, akkor URES PASSWORD-del probaljuk meg majd!!!!!!)
  * amikor megkerdezi hogy akarunk e keresni benne elrejtett fajlot es ranyomunk hogy igen
  * utana kerni fog password-ot is
  * PROBALJUK MEG URES PASSWORD-del!!!!!!!
  * HA VAN BENNE VALAMI !!!
    * steghide extract -sf image.jpg (es ami benne van azt kipakolja majd)
* foremost -i image.jpg
* binwalk image.jpg
* binwalk -e image.jpg
## cracking (bruteforcing) password of image stego info
* stegcracker image1.jpg rockyou.txt
