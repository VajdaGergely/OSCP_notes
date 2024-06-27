# Web servers vs reverse proxies
* bizonyos ertelemben a web serverek reverse proxykent mukodnek hogyha ugy hasznaljuk oket
* kb hasonlo oldalon mukodnek es el vannak valasztva a masik oldaltol a framework oldaltol
* valojaban minden web stack-nel valszeg ket oldal van
  * az egyik ahol a HTTP requestek es response-ok menedzselese tortenik
  * es a masik oldal, ahol a kod futtatas tortenik, az alkalmazas logikai dolgai
# Protocols for linking web server and application server
* legacy solution
  * CGI
    * web servers passed requests to cgi scripts
    * cgi scripts runned by code interpreter (php, perl, ...) or by OS stuff if it is compiled code (C, C++)
* new solutions
  * FastCGI
  * AJP (Apache JServ)
  * WSGI
  * Here not cgi scripts runned but more complex requests sent (like api requests) to the application server
    * Application server probably is a kind of big framework that has an interpreter to run code and to other complex tasks
# Web servers, web frameworks, WSGI
* web servers
  * e.g. Apache, Nginx
  * can run on port 80
  * handle static files efficiently
  * no advanced scripting capabilities (just on the CGI level)
* application servers with WSGI specification
  * e.g. Gunicorn,
  * keep 'scripting language interpreter in memory'
  * can run script like python or php
  * can run on multiple processes, can do multithreading
  * can not run on port 80
  * not good at handling static files
* web frameworks
  * e.g. flask, django, laravel, CakePhp, Ruby on rails
* programming languages
  * PHP, Python
* CGI
  * an interface specification that enables web servers to execute an external program to process HTTP requests
  * often written in a scripting language (CGI scripts), also can include compiled code (e.g. C, C++ code)
  * every request make a new process created and destroyed after run
    * this makes an overhead on CPU and memory
    * solutions to CGI computational overhead
      * using compiled code (C, C++) instead of scripts ran by interpreter (PHP, Perl, Python)
      * web server extensions
        * Apache modules (mod_perl, mod_php, mod_python)
        * Netscape (NSAPI plugins)
        * IIS (ISAPI plugins)
        * These allow long-running application processes handling more request nad hosted within the web server
          * ?? csak azt interpreter resze - a konkret kod futtatas megy at a php-nek, python-nak, ... es a process ami futtatja oket, az folyamatosan fut... ??
      * FastCGI, SCGI, AJP
        * one long running process can handle more reuquest
        * run separately from the web server
        * web server handles the static content directly
        * dinamic contents are handled by the CGI stuff
      * WSGI (Web Server Gateway Interface) (written in Python)
        * Apache mod_wsgi
        * Gunicorn web server (with nginx and a python framework like django)
* WSGI
  * has two sites
    * server/gateway side
      * full web server (Apache, Nginx)
      * or lightweight application server that can communicate with a web server (itt valszeg kulso komponens lesz a web server ami kivul van a WSGI-n)
    * application/framework side
      * valami ami python kodot tud lefuttatni - python framework vagy mas egyeb, amiben ott van a python interpreter
  * web frameworks that support WSGI
    * Django, Flask, Gunicorn, mod_wsgi (can be used with Apache), Werkzeug
  * mi a lenyeg a mi ertelmezesunk szerint
    * valszeg az a lenyege a WSGI-nek, hogy python kodot lehessen minden szarral futtatni
    * es akkor kell valami framework-szeru cucc
      * ami alapszinten futtatni kepes a python kodot (mint egy sima python cgi)
      * magasabb szinten meg kepes mindenfele middleware funkciokat is ellatni - routing, load balancing, ...
        * ez valszeg lehet beepitett cucc a framework-be vagy lehet valami kulso cucc is, ami egy kulonallo komponens
      * illetve mindenfele modern web alkalmazas kenyelmes funkcioit is kepes kezelni
        * bonyolult cookie meg session kezeles - komplett user es role management, elore megirt login, logout, stb funkciok
        * mvc, crud, omr, egyeb magasabb szintu webes cuccok elore elkeszitve...
    * emelett kell valami web server oldali cucc is
      * ez lehet valami kulso cucc is - apache, stb
      * vagy lehet talan valami beepitett cucc is, ha van olyan framework ami ilyet is tud (vagy ha van built in egyszeru - ami pont eleg)
      * illetve lehet kulon valami load balancer - proxy - cache server - stb berakva vagy a web server-be vagy attol kulon szedve tovabbi komponenskent
