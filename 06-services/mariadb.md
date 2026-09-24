# MariaDB

MariaDB is a database server.

A database server is a program that:

```
MariaDB server
      │
      ├── stores data
      ├── manages databases
      ├── manages users/permissions
      └── accepts client connections
```
The important word here is **server**.

It is a **running process**.

Conceptually:
```
Client
   │
   │ SQL connection
   ▼
MariaDB server
   │
   ▼
Database files
```

## MariaDB Linstens
MariaDB listens for connections
```
Conceptually:

MariaDB server
       │
       │ listens
       ▼
     port 3306
```
A client connects using something conceptually like:
```
<HOST>:<PORT>
```

For example:
```
localhost:3306
```
When the client and MariaDB server are on the same machine.

## Databse Vs Server
MariaDB is a **database server software**, not a database. Inside that server, you can have multiple databases:
```
MariaDB server
	  │
	  ├── database1
	  ├── database2
	  └── database3
```
A database is a collection of data, while a database server is a program that manages databases and provides access to them.