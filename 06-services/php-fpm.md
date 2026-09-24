# PHP-FPM

## What is PHP?
PHP is a popular general-purpose scripting language that is especially suited to web development.

Suppose you have:
```
<?php
echo "Hello from PHP";
?>
```
Save it as:
```
index.php
```

You can execute it directly with PHP:
```
php index.php
```
The flow is:
```
index.php
    │
    ▼
PHP
    │
    ▼
executes code
    │
    ▼
Hello from PHP
```
No web server is required for this simple example.


## What is PHP-FPM?

PHP-FPM means:

*PHP FastCGI Process Manager*

PHP-FPM is a software (a process manager) that runs PHP code and listens for requests from a web server. It is an implementation of FastCGI for PHP.

Conceptually:
```
Web server
   │
   │ sends request
   ▼
PHP-FPM
   │
   ▼
executes code
   │
   ▼
Hello from PHP
```
