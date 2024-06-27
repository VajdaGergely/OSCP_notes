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
