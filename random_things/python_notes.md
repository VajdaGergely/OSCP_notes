# install package inside virtualenv
<pre>
$ git clone <repo> <repodir>
$ cd &lt;repodir&gt;
$ virtualenv .venv
$ . .venv/bin/activate
$ pip install -e .
</pre>
# Versions
* 1.X -> mar nem hasznaljuk
* 2.X, pl 2.7 -> nem supportalt, de meg sok helyen hasznaljak, regi exploitokhoz pl
* 3.X -> az uj python 3.10 alatt mar nem supportolt, afolott igen
# Python implementations
* CPython -> ez a sima python
* Jython -> python kodbol java byte kod lesz es jvm-ben tud lefutni
* IronPython -> >python kodbol .net byte kod lesz es a CL.exe tudja lefuttatni
* Cython -> python kodbol c kod lesz, es abbol meg gondolom sima exe fajl...
* PyPy -> egy mas fajta python futtatasi kornyezet
  * gyorsabban fut le a python kod, mert nem iterpreter-ben fut hanem JIT compilerben 
  * a sima python-nak egy masik iranyba fejlesztett valtozata
  * a mar unsupported python verziokhoz nyujtanak tovabbi supportot es adnak ki hozza frissiteseket
    * Python 2.7, 3.7, 3.8 and 3.9
  * de ez nem a hivatalos python
  * a kompatibilitas altalaban jo, de van hogy nem tokeletes
  * ha valami exploit-hoz ez kell, akkor ezzel futtassuk majd es ne a sima pythonnal
# PyPI
* official python repository for downloading python modules
# pip
* official python package manager
* it uses the official PyPI repo to download packages
* it has version 2, 3 and 3.4
* there is no pip for Python1 (we don't use Python 1 at all)
# virtualenv
* lehet vele csinalni virtualis kornyezetet
* venv -> a python3-as uj valtozata
  * bekerult a standard library-be es mar nem kulon modul
  * kevesebb dolgot tud mint a virtualenv
  * a virtualenv meg mindig nepszerubb
  * Python2-hoz pl nem hasznalhato
