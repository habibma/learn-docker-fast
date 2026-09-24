# Networks

You already learned Docker networking separately.

Now **Compose** can create the network for you.

```yaml
version: '3.8'

networks:
  internal-network:
	driver: bridge

services:

  mariadb:
    networks:
      - internal-network

  wordpress:
    networks:
      - internal-network

  nginx:
    networks:
      - internal-network
```
The resulting architecture is:
```
               internal-network
        ┌──────────────┼──────────────┐
        │              │              │
        ▼              ▼              ▼
    mariadb        wordpress        nginx
```
Because they're on the same Docker network, they can communicate using service names.

For example:
```diagram
wordpress → mariadb:3306
```
and:
```diagram
nginx → wordpress:9000
```