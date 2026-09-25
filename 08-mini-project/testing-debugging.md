# Testing and Debugging
Open:
```
http://localhost:8080
```
NGINX receives the request:
```
Browser
   │
   │ :8080
   ▼
NGINX
```
If the request requires PHP:
```
NGINX
   │
   │ FastCGI
   ▼
WordPress / PHP-FPM
```
If WordPress needs database data:
```
WordPress
   │
   │ SQL
   ▼
MariaDB
```
The response then travels back:
```
MariaDB
   │
   ▼
WordPress / PHP-FPM
   │
   ▼
NGINX
   │
   ▼
Browser
```
## Verify the Network

You can inspect the network created by Compose:
```
docker network ls
```
Then inspect the project network:
```
docker network inspect <network-name>
```
You should see the three containers connected to the same network.

You can also test Docker DNS from inside the WordPress container:
```
docker compose exec wordpress getent hosts mariadb
```
Docker should resolve:
```
mariadb
```
to the MariaDB container's IP address.

This demonstrates the networking concept we learned earlier:

> Containers communicate using service names, not localhost.