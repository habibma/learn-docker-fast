# Ports

You already learned internal vs host ports.

Compose lets you define published ports.

For example:
```yaml
nginx:
  ports:
    - "443:443"
```
Conceptually:
```text
Host
 │
 │ :443
 ▼
NGINX container
 │
 └── :443
```
But notice something important:

You don't necessarily need:
```yaml
mariadb:
  ports:
    - "3306:3306"
```
for WordPress to communicate with MariaDB.

Why?

Because:
```text
WordPress
    │
    │ Docker network
    ▼
mariadb:3306
```
works internally.

You only publish ports when something outside the Docker network needs access.

For the Inception architecture, NGINX is the public entry point:
```text
Internet
   │
   ▼
Host :443
   │
   ▼
NGINX
```
while internal communication stays internal:
```text
NGINX → wordpress:9000
WordPress → mariadb:3306
```