# Understand the Services
## MariaDB
```
mariadb:
  build: ./mariadb
```
Compose builds the image using:
```
mariadb/Dockerfile
```
The environment variables configure the initial database:
```Dockerfile
environment:
  MYSQL_ROOT_PASSWORD: root
  MYSQL_DATABASE: wordpress
  MYSQL_USER: wordpress
  MYSQL_PASSWORD: wordpress
```
The database files are stored in:
```Dockerfile
volumes:
  - mariadb-data:/var/lib/mysql
```
Therefore, the database survives container recreation.

## WordPress
```Dockerfile
wordpress:
  build: ./wordpress
```
The WordPress container receives its database configuration through environment variables:
```Dockerfile
environment:
  WORDPRESS_DB_HOST: mariadb:3306
  WORDPRESS_DB_USER: wordpress
  WORDPRESS_DB_PASSWORD: wordpress
  WORDPRESS_DB_NAME: wordpress
```
Notice:
```text
WORDPRESS_DB_HOST
        │
        ▼
mariadb:3306
```
WordPress doesn't use:
```
localhost
```
because MariaDB is running in another container.

It uses the Compose service name:
```text
mariadb
```
## NGINX
NGINX is the only service that publishes a port to the host:

```Dockerfile
ports:
  - "8080:80"
```
This means:
```text
Host port 8080
      │
      ▼
Container port 80
      │
      ▼
NGINX
```
You can therefore access the application through:
```
http://localhost:8080
```
NGINX also needs access to the WordPress files:

```Dockerfile
volumes:
  - wordpress-data:/var/www/html:ro
```
The :ro means read-only.

NGINX only needs to read the files. WordPress is responsible for modifying them.

## Configure the Network

All three services are connected to:
```
networks:
  - internal-network
```
Compose creates the network:
```
networks:
  internal-network:
    driver: bridge
```
The resulting architecture is:
```text
                 internal-network
                       │
        ┌──────────────┼──────────────┐
        │              │              │
        ▼              ▼              ▼
     NGINX          WordPress       MariaDB
       │               │              │
       │               │              │
       └─────── HTTP ──┘              │
                       │              │
                       └──── SQL ─────┘
```
Docker's internal DNS allows the services to use their Compose service names:
```text
nginx      → wordpress:9000
wordpress  → mariadb:3306
```
## Configure Persistent Volumes

We define two named volumes:
```Dockerfile
volumes:
  mariadb-data:
  wordpress-data:
```
The MariaDB volume stores database files:
```text
mariadb-data
      │
      ▼
/var/lib/mysql
```
The WordPress volume stores the WordPress application files:
```text
wordpress-data
      │
      ▼
/var/www/html
```
The important idea is:
```text
Container
    │
    │ can be removed
    ▼
Volume
    │
    │ remains
    ▼
Persistent data
```