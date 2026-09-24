# NGINX

Now learn NGINX by itself.

Start with the simplest possible flow:
```
Browser
   │
   │ sends request
   ▼
NGINX
   │
   ▼
executes code
   │
   ▼
index.html
   │
   ▼
Hello from NGINX
```

## Static HTML
In static HTML, the flow looks like this:
```
Browser
   │
   │ sends request (HTTP request)
   ▼
NGINX
   │
   ▼
index.html
   │
   ▼
Hello from NGINX
``` 
If the request is encrypted with HTTPS, the flow looks like this:
```
Browser
   │ encrypted HTTPS
   ▼
NGINX
   │
   ▼
SSL/TLS :  decrypts/handles TLS
   │
   ▼
index.html
   │
   ▼
Hello from NGINX
```


## NGINX + PHP-FPM
When NGINX is used with PHP-FPM, the flow looks like this:
```
Browser
   │ sends request
   ▼
NGINX
   │
   |  static request
   ├──────────────────► index.html|
   |
   │ PHP request to PHP-FPM
   ▼
PHP-FPM
   │ executes code
   ▼
Hello from PHP
```


## Wrap up
Imagine in the NGINX server, there is a file named `index.php`. On the browser, you request `index.php`. Thiis is what happens:
```
Browser requests /index.php
        │
        ▼
NGINX receives request
        │
        ▼
NGINX recognizes PHP request
        │
        │ FastCGI
        ▼
PHP-FPM receives request
        │
        ▼
PHP executes index.php
        │
        ▼
"Hello from PHP"
        │
        ▼
NGINX sends response
        │
        ▼
Browser
```