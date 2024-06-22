# Archiving and extracting files
## Archiving files
* zip -r file.zip files/ (-r means recursive, without it, the empty folder will be zipped)
* zip -r -e -P Password1. file.zip files/ (-e means encrypt, -P is the password)
## Extracting files
|file|command|extra info|
|----|-------|----------|
|.zip|unzip file.zip|zip file can extracted with 7z as well|
|.gz|gzip -dk file.gz||
|.tar|tar -xvf file.tar||
|.7z|7z x file.7z||
|.tar.zst|tar --zstd -xvf file.tar.zst|**double dash on 'zstd' single dash on 'xvf'!!**|
