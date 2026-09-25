# Implementation
Now that we understand the architecture, we can turn it into a working Docker project.

We will build three services:

```text
NGINX
  │
  │ FastCGI
  ▼
WordPress + PHP-FPM
  │
  │ SQL
  ▼
MariaDB
```
Each service will have a clear responsibility:

NGINX — web server and public entry point
WordPress + PHP-FPM — PHP application
MariaDB — database

Docker Compose will connect the services through a shared network and provide persistent volumes.

## 1. Create the project structure
Start with the following structure:
```text
mini-project/
│
├── mariadb/
│   └── Dockerfile
│
├── wordpress/
│   └── Dockerfile
│
├── nginx/
│   ├── Dockerfile
│   └── nginx.conf
│
└── compose.yml
```
We will build each service separately and then connect them with Docker Compose.

## 2. Create the MariaDB Image

Create:
```
mariadb/Dockerfile
```
For this project, we can build our MariaDB image from the official MariaDB image:
```
FROM mariadb:10.11
```
We don't need to put database credentials directly into the Dockerfile.

The configuration belongs in `compose.yml`.

This gives us an important separation:
```
Dockerfile
    │
    ▼
Image
```
```
compose.yml
    │
    ├── environment
    ├── networks
    ├── volumes
    └── ports
```

## 3. Create the WordPress Image

Create:
```
wordpress/Dockerfile
```
Use the official WordPress image with PHP-FPM:
```
FROM wordpress:php8.2-fpm
```
The image already provides:
```
WordPress
PHP
PHP-FPM
the required WordPress runtime
```
We therefore don't need to manually install PHP or copy WordPress into the image.

The database configuration will be provided through Compose.

## 4. Create the NGINX Image

Create:
```
nginx/Dockerfile
```
And write the following in the Dockerfile:
```
FROM nginx:alpine

COPY nginx.conf /etc/nginx/conf.d/default.conf
```
NGINX will use the configuration file to:

- listen for HTTP requests
- serve WordPress files
- forward PHP requests to PHP-FPM

Create:
```
nginx/nginx.conf
```
with:
```
server {
    listen 80;
    server_name localhost;

    root /var/www/html;
    index index.php index.html;

    location / {
        try_files $uri $uri/ /index.php?$args;
    }

    location ~ \.php$ {
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        fastcgi_pass wordpress:9000;
    }
}
```
Notice this:
```
fastcgi_pass wordpress:9000;
```
`wordpress` is the Docker Compose service name.

Docker's internal DNS resolves it to the WordPress container.

## 5. Create the Docker Compose File

Now we can define the complete application in compose.yml.
```Dockerfile
services:

  mariadb:
    build: ./mariadb
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress
    volumes:
      - mariadb-data:/var/lib/mysql
    networks:
      - internal-network

  wordpress:
    build: ./wordpress
    environment:
      WORDPRESS_DB_HOST: mariadb:3306
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress
      WORDPRESS_DB_NAME: wordpress
    volumes:
      - wordpress-data:/var/www/html
    networks:
      - internal-network

  nginx:
    build: ./nginx
    ports:
      - "8080:80"
    volumes:
      - wordpress-data:/var/www/html:ro
    networks:
      - internal-network

volumes:
  mariadb-data:
  wordpress-data:

networks:
  internal-network:
    driver: bridge
```
Let's break this down.
