# Environment variables

Services often need configuration.

For example, MariaDB needs information such as:

- database name
- database user
- database password
- root password

You can provide configuration through environment variables.

Conceptually:
```
environment:
  MYSQL_DATABASE: ...
  MYSQL_USER: ...
  MYSQL_PASSWORD: ...
```
Then the container's startup process can read those values.

Think of environment variables as:
```
Configuration
      │
      ▼
Container environment
      │
      ▼
Startup script/application
```
Later, you'll need to decide carefully which values should be placed directly in the Compose file and which should come from an environment file or Docker secrets, depending on the Inception requirements.

Don't worry about that detail yet.