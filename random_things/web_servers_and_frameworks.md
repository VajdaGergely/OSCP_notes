# History of web frameworks
* web servers + static pages
  * html files, pictures, ...
* web servers + cgi scripts + applications servers
  * dynamic web content
  * forms, url querystrings
  * application servers
    * binary code (C, C++ compiled to exe) -> OS futtatja
    * scripts (perl, python, ruby) -> language specific interpreter (e.g. python interpreter)
* web severs + web frameworks
  * web applications
  * multimedia, client side scripts, users, sessions, ...
  * IDE-s
    * integrated server / language development environments with new web specific languages
    * ColdFusion, PHP, ASP
    * libraries for specific tasks
  * Full stack frameworks
    * mindent is tudnak
    * 
# Web stack
* web server side
  * web server, forward and reverse proxies, load balancers, cache servers, WAFs, ...
* application side
  * cgi scripts / web frameworks
* other components
  * databases, ...
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
# Misc
<pre>
Note the subtle difference in terminology:

Django is a web framework. It lets you build the core web application that powers the actual content on the site. It handles HTML rendering, authentication, administration, and backend logic.

Gunicorn is an application server. It translates HTTP requests into something Python can understand. Gunicorn implements the Web Server Gateway Interface (WSGI), which is a standard interface between web server software and web applications.

Nginx is a web server. It’s the public handler, more formally called the reverse proxy, for incoming requests and scales to thousands of simultaneous connections.

Part of Nginx’s role as a web server is that it can more efficiently serve static files. This means that, for requests for static content such as images, you can cut out the middleman that is Django and have Nginx render the files directly.



https://realpython.com/django-nginx-gunicorn/







-------------------------------------
https://www.reddit.com/r/djangolearning/comments/vuxn9w/distinction_between_what_django_gunicornwsgi_and/

-------------------------------------
Distinction between what django, gunicorn(wsgi) and nginx do in deployment of django app.

Im new to deployment and I'm having hard time understanding what is the purpose nginx, gunicorn and django.

If python itself can serve like the one during development then why require any thing like nginx or gunicorn? All the request to port 80 can be made to be handled by a specific python process.

If the standard way of serving webpage in internet is nginx then why need gunicorn and wsgi? Can't nginx serve what it can, like all the static files and forward the request to the running python process when it can't handle that URL string?

Why is gunicorn needed in between nginx and python process running django app?

I could not find any easy to understand post online so you can post them if you think that will help me understand these concepts.







"Python itself" can not serve your Django site. You need to run a server, which can be either the Django development server or a WSGI server like Gunicorn.

Nginx and Gunicorn are needed to run your Django site in production. The main reason is that they provide much better performance then if you would run your site with the Django development server, e.g. Gunicorn will run with multiple so called workers, which allow handling requests in parallel, while the Django development server can only handle one request at a time. Nginx is optimized to serve static content for many requests in parallel, which again the Django development server can not.

So while it is technically possible to use the development Django server to run your site it is not recommended because you get poor performance if you have more than one user at the same time.






-------------------------------------
NodeJS Express.js Javascript
NodeJS Socket.IO Javascript


-------------------------------------
Nodejs soroknal.
A nodejs legyen elol!!!!
Mert az a webserver....

-------------------------------------
Nodejs sorokhoz a javascriptet is rakjuk majd oda a vegere


-------------------------------------
Valojaban!!!!!!

A werkzeug az csak egy library!!!!

A flask az a framework neve es igazabol osszefog tobb dolgot - pl a werkezeug is koztuk van mas egyeb dolgok is.

A flask micro framework - mert csak alap dolgokat tud es nem annyi mindent mint egy full stack framework mint pl a django.

Amikor egy flask app-ot elinditok a builtin development serveren. Akkor a server headerben a 'werkzeug' lesz!!!!
Tehat innen tudom hogy egy dev server fut es nem rendes server.
A werkzeug nev mogott viszont mindig a flask van igazabol!

-------------------------------------
Werkzeug is primarily a library, not a web server, although it does provide a simple web server for development purposes. That development server is what's providing that Server: header.

https://stackoverflow.com/questions/37004983/what-exactly-is-werkzeug

-------------------------------------
Nginx werkzeug flask python
Gunicorn flask python
Nginx gunicorn flask python
Nginx werkzeug python

-------------------------------------
https://testdriven.io/blog/what-is-werkzeug/

Werkzeug fuggvenyekkel is lehet python kodot irni flask nelkul.

De webserverlent nem uzemel ez a szar.

-------------------------------------
A werkzeug elvileg a flask-nak a resze. Gyakorlatilag csak egy python modul - ami a wsgi funkciokat ellatja. Es egybe van a flaskkal - nem egy kulon komponens.

A gunicorn szinten egy wsgi cucc.... de komolyabb http cuccokat is el tud latni. A werkzeugnak csak builtin test http server-t tartalmaz.


-------------------------------------
Flask Dependencies
You may have already noticed, but every time you install Flask, you also install the following dependencies:

Blinker
Click
ItsDangerous
Jinja
MarkupSafe
Werkzeug
Flask is a wrapper around all of them.

$ pip install Flask
$ pip freeze

blinker==1.7.0
click==8.1.7
Flask==3.0.0
itsdangerous==2.1.2
Jinja2==3.1.2
MarkupSafe==2.1.3
Werkzeug==3.0.1  # !!!

What is Werkzeug?
Werkzeug is a collection of libraries that can be used to create a WSGI (Web Server Gateway Interface) compatible web application in Python.

A WSGI (Web Server Gateway Interface) server is necessary for Python web applications since a web server cannot communicate directly with Python. WSGI is an interface between a web server and a Python-based web application.

Put another way, Werkzeug provides a set of utilities for creating a Python application that can talk to a WSGI server, like Gunicorn.

Werkzeug provides the following functionality (which Flask uses):

Request processing
Response handling
URL routing
Middleware
HTTP utilities
Exception handling
It also provides a basic development server with hot reloading.


-------------------------------------
Flask use werkzeug as default WSGI container, but it cannot support
production requirement and leads to crash in some cases. So use
gunicorn instead of it.

-------------------------------------
Werkzeug szinten egy wsgi cucc!!!


-------------------------------------
Microframeworks

Bottle for Python
Camping for Ruby
Express.js for Node.js
Falcon for Python
Flask for Python
Scalatra for Scala
Lumen for PHP
Slim for PHP
Silex for PHP
Sinatra for Ruby

-------------------------------------
A microframework is a term used to refer to minimalistic web application frameworks. It is contrasted with full-stack frameworks.

It lacks most of the functionality which is common to expect in a full-fledged web application framework, such as:

Accounts, authentication, authorization, roles
Database abstraction via an object-relational mapping
Input validation and input sanitation
Web template engine
Typically, a microframework facilitates receiving an HTTP request, routing the HTTP request to the appropriate function and returning an HTTP response. Microframeworks are often specifically designed for building the APIs for another service or application.

-------------------------------------
Flask builtin dev server

Gunicorn Flask Python

uWSGI Flask Python

Nginx uWSGI Flask Python

Nginx Gunicorn Flask Python





-------------------------------------
Php cli tool built in test server
Apache (nothing) (static files)

Apache mod_php (pure)Php
Apache mod_python (pure)Python
Apache mod_perl (pure)Perl
(Proxy) Apache mod_php (pure)Php
Nginx (???) (pure)Php

Apache mod_php Laravel Php
Apache mod_php CakePhp Php
...
Nginx (???) Laravel Php
...

->JSP, Apache Tomcat, Jetty, AJP

Express NodeJS

Ruby On Rails

IIS ASP.NET C#

(Django runserver) Django Python
Apache mod_wsgi Django Python
Apache Gunicorn Django Python
Nginx Gunicorn Django Python
Nginx uWSGI Django Python



-->Flask




-------------------------------------
Node.js allows the creation of web servers and networking tools using JavaScript and a collection of "modules" that handle various core functionalities. Modules are provided for file system I/O, networking (DNS, HTTP, TCP, TLS/SSL or UDP), binary data (buffers), cryptography functions, data streams and other core functions. Node.js's modules use an API designed to reduce the complexity of writing server applications.


Node.js is primarily used to build network programs such as web servers. The most significant difference between Node.js and PHP is that most functions in PHP block until completion (commands execute only after previous commands finish), while Node.js functions are non-blocking (commands execute concurrently or even in parallel,[improper synthesis?] and use callbacks to signal completion or failure).


Node.js enables development of fast web servers in JavaScript using event-driven programming. Developers can create scalable servers without using threading by using a simplified model that uses callbacks to signal the completion of a task.[page needed] Node.js connects the ease of a scripting language (JavaScript) with the power of Unix network programming.

Node.js was built on top of Google's V8 JavaScript engine since it was open-sourced under the BSD license, and it contains comprehensive support for fundamental protocols such as HTTP, DNS and TCP.

-------------------------------------
Express.js, or simply Express, is a back end web application framework for building RESTful APIs with Node.js, released as free and open-source software under the MIT License. It is designed for building web applications and APIs. It has been called the de facto standard server framework for Node.js.

-------------------------------------
Node.js is a cross-platform, open-source JavaScript runtime environment that can run on Windows, Linux, Unix, macOS, and more. Node.js runs on the V8 JavaScript engine, and executes JavaScript code outside a web browser.

-------------------------------------
Some web application frameworks include simple HTTP servers. For example the Django framework provides runserver, and PHP has a built-in server. These are generally intended only for use during initial development. A production server will require a more robust HTTP front-end such as one of the servers listed here.
</pre>
