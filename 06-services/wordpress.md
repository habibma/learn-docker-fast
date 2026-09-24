# What is WordPress?
WordPress is a free and open-source content management system (CMS) written in PHP. In one sentence, It is a PHP application.

If a website is built with WordPress, it means that the website is using WordPress as its CMS to manage and publish content. When a user visits a WordPress-powered website, they are interacting with the WordPress application running on a web server. The application retrieves content from a database and generates HTML pages that are sent to the user's web browser for display.

It is the workflow of a WordPress website:
```
Browser
   │ sends request
   ▼
NGINX
   │ sends request to PHP-FPM
   ▼
PHP-FPM
   │ executes WordPress code
   ▼
WordPress
   │ retrieves content from database
   ▼
Database
   │ sends content back to WordPress
   ▼
WordPress
   │ generates HTML pages
   ▼
NGINX
   │ sends HTML pages back to browser
   ▼
Browser
```

Now, let's jump in back to Docker and Compose.