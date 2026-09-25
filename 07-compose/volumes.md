# Volumes

You also learned `volumes` separately.

Instead of managing volumes manually, **Compose** can define persistent storage:
```yaml
volumes:
  mariadb-data:
  wordpress-data:

networks:
  internal-network:
	driver: bridge

services:

  mariadb:
    volumes:
      - mariadb-data:/var/lib/mysql

  wordpress:
    volumes:
      - wordpress-data:/var/www/html
```
Conceptually:
```diagram
                     Docker
                       │
              ┌────────┴────────┐
              │                 │
       mariadb-data      wordpress-data
              ▲                 ▲
              │                 │
         MariaDB           WordPress
         container         container
```
So if the MariaDB container is recreated:
```
Old MariaDB container
        │
        │ deleted
        ▼
       ❌
```
the volume remains:
```
mariadb-data
      │
      ▼
database survives
```
A new container can mount it again.