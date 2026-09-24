# Basic SQL practice
Once you're inside the MariaDB client, create a database:
```sql
CREATE DATABASE my_database;
```
It creates a database called `my_database`. You can check that it has been created by running:
```sql
SHOW DATABASES;
```

Now create a user:
```sql
CREATE USER 'my_user'@'%' IDENTIFIED BY 'my_password';
```
This creates a user called `my_user` with the password `my_password`.  The `@'%'` part means that the user can connect from any host.

Now grant the user access to the database:
```sql
GRANT ALL PRIVILEGES ON my_database.* TO 'my_user'@'%';
```
This grants the user `my_user` all privileges on the database `my_database`.
`.*` means all tables in the database.

You can check that the user has been created by running:
```sql
SELECT user, host FROM mysql.user;
```

Finally, flush the privileges to make sure that they are saved:
```sql
FLUSH PRIVILEGES;
```
