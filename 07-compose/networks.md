# Networks

You already learned **Docker networking** separately.

Instead of managing networks manually, **Compose** can create the network for you.

```yaml
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
```text
               internal-network
        ┌──────────────┼──────────────┐
        │              │              │
        ▼              ▼              ▼
    mariadb        wordpress        nginx
```
Because they're on the same Docker network, they can communicate using service names.

For example:
```text
wordpress → mariadb:3306
```
and:
```text
nginx → wordpress:9000
```