# Architecture

 Architecture

The mini-project will build a small WordPress application using four services:

- **NGINX** — web server and public entry point
- **WordPress / PHP-FPM** — PHP application
- **MariaDB** — database server

The services will run in separate Docker containers and communicate through a Docker network.

The architecture follows an important Docker principle:

> **Each container should have one main responsibility.**

The final architecture looks like this:

```text
                         Browser
                            │
                            │ HTTP / HTTPS
                            ▼
                         NGINX
                            │
                            │ FastCGI
                            ▼
                    WordPress / PHP-FPM
                            │
                            │ SQL
                            ▼
                         MariaDB
                            │
                            ▼
                    Persistent Volume
```
### NGINX

NGINX is the public-facing web server.

It will:

- accept HTTP/HTTPS requests from the browser
- serve static files when appropriate
- forward PHP requests to PHP-FPM

NGINX is the only service that needs to be directly accessible from the host.

For example:
```
Host
  │
  │ :8080
  ▼
NGINX container
```
NGINX then communicates with PHP-FPM through the internal Docker network.

### WordPress / PHP-FPM

WordPress is the PHP application that powers the website.

PHP-FPM is the process manager that executes the PHP code.

The WordPress container will:

- contain the WordPress application
- run PHP-FPM
- receive PHP requests from NGINX
- execute WordPress code
- communicate with MariaDB when WordPress needs database data

PHP-FPM will listen on port 9000 inside the container.
```text
NGINX
  │
  │ FastCGI
  ▼
PHP-FPM :9000
  │
  │ executes
  ▼
WordPress
```
PHP-FPM does not provide HTTP responses directly to the browser. NGINX handles HTTP and communicates with PHP-FPM using FastCGI.

### MariaDB

MariaDB is the database server.

It will:

- store WordPress data
- receive SQL queries from WordPress
- process queries
- return query results

MariaDB will listen on port 3306 inside the Docker network.
```text
WordPress
    │
    │ SQL
    ▼
MariaDB :3306
    │
    ▼
Database files
```
MariaDB does not need to publish port 3306 to the host because only the WordPress container needs to access it.

## How they communicate

The services communicate through a shared Docker network.

The basic communication flow is:
```text
Browser
   │
   │ HTTP / HTTPS
   ▼
NGINX
   │
   │ FastCGI
   ▼
PHP-FPM / WordPress
   │
   │ SQL
   ▼
MariaDB
```
Inside the Docker network, containers do not need to use localhost to reach each other.

Instead, they use the service name as the hostname:
```
NGINX      → wordpress:9000
WordPress  → mariadb:3306
```
Docker's internal DNS resolves these service names to the corresponding container IP addresses.

For example:
```
wordpress
    │
    ▼
Docker DNS
    │
    ▼
WordPress container IP
```
This is one of the important concepts we learned in the Networking section.

## Network Diagram
```text
                         Browser
                            │
                            │ HTTP / HTTPS
                            ▼
                    ┌────────────────┐
                    │     NGINX      │
                    │   Web Server   │
                    └───────┬────────┘
                            │
                            │ FastCGI
                            │
                            ▼
                    ┌────────────────┐
                    │   WordPress    │
                    │   PHP-FPM      │
                    └───────┬────────┘
                            │
                            │ SQL
                            │
                            ▼
                    ┌────────────────┐
                    │    MariaDB     │
                    │    Database    │
                    └───────┬────────┘
                            │
                            ▼
                    ┌────────────────┐
                    │     Volume     │
                    │ Persistent Data│
                    └────────────────┘
```
The complete architecture looks like this:
```text
                         Browser
                            │
                            │ HTTP / HTTPS
                            ▼
                    ┌────────────────┐
                    │     NGINX      │
                    │   Web Server   │
                    └───────┬────────┘
                            │
                            │ FastCGI
                            │
                            ▼
                    ┌────────────────┐
                    │   WordPress    │
                    │   PHP-FPM      │
                    └───────┬────────┘
                            │
                            │ SQL
                            │
                            ▼
                    ┌────────────────┐
                    │    MariaDB     │
                    │    Database    │
                    └───────┬────────┘
                            │
                            ▼
                    ┌────────────────┐
                    │     Volume     │
                    │ Persistent Data│
                    └────────────────┘
```
All three services communicate through the same Docker network.

Only NGINX is exposed to the host.

## Containers

Each service runs in its own container:
```text
┌─────────────────────────┐
│     NGINX container     │
│                         │
│         nginx           │
└────────────┬────────────┘
             │
             │ Docker network
             │
┌────────────▼────────────┐
│   WordPress container   │
│                         │
│       PHP-FPM           │
│       WordPress         │
└────────────┬────────────┘
             │
             │ Docker network
             │
┌────────────▼────────────┐
│    MariaDB container    │
│                         │
│       mariadbd          │
└────────────┬────────────┘
             │
             │
┌────────────▼────────────┐
│      MariaDB volume     │
│                         │
│     Database files      │
└─────────────────────────┘
```
Each container has a specific responsibility:
```text
NGINX       → HTTP / web server
WordPress   → PHP application
MariaDB     → Database
```
This keeps the services independent and makes them easier to build, run, replace, and debug.

## Volumes

Containers are ephemeral by nature.

If the MariaDB container is removed, its filesystem can disappear with it.

Database data therefore needs persistent storage.

We will attach a Docker volume to MariaDB:
```text
MariaDB container
       │
       ▼
MariaDB volume
       │
       ▼
Database files
```
The volume exists independently of the container.

Therefore:
```text
Remove container
       │
       ▼
Container disappears
       │
       ▼
Volume remains
       │
       ▼
Database data remains
```
The WordPress application files may also require persistent storage depending on how the project is configured.

## Ports

A port can have two different roles in this architecture:

### Host ports
A host port makes a container service accessible from outside Docker.

For example:
```
Host
 │
 │ :8080
 ▼
NGINX :8080
```
Only NGINX needs this kind of access.

### Container ports

Services can communicate with each other using their internal container ports.

For example:
```
NGINX     → wordpress:9000
WordPress → mariadb:3306
```
These ports do not need to be published to the host.

Therefore, we do not need:
```text
9000:9000
3306:3306
```
just because PHP-FPM and MariaDB use those ports.

The important distinction is:
Host access:
```
localhost:8080
      │
      ▼
    NGINX
```

Container-to-container:
```
wordpress:9000
mariadb:3306
```
## Data Flow
Let's follow a request through the entire application.
1. Browser sends a request
```text
Browser
   │
   │ HTTP
   ▼
NGINX
```
2. NGINX receives the request

NGINX determines how the request should be handled.

If it can serve the requested resource directly, it does so.

If the request requires PHP execution, NGINX forwards it to PHP-FPM.
```text
NGINX
   │
   │ FastCGI
   ▼
PHP-FPM
```
3. PHP-FPM executes WordPress

PHP-FPM runs the WordPress PHP application.
```text
PHP-FPM
   │
   ▼
WordPress
```
4. WordPress needs database data

WordPress sends an SQL query to MariaDB.
```text
WordPress
   │
   │ SQL
   ▼
MariaDB
```
5. MariaDB returns the result
```text
MariaDB
   │
   │ Query result
   ▼
WordPress
```
6. WordPress generates the response

PHP-FPM executes the necessary PHP code and produces the response.
```text
WordPress
   │
   ▼
PHP-FPM
   │
   │ FastCGI response
   ▼
NGINX
```
7. NGINX responds to the browser
```text
NGINX
   │
   │ HTTP response
   ▼
Browser
```
Complete Request Flow

Putting everything together:
```text
                         Browser
                            │
                            │ HTTP / HTTPS
                            ▼
                         NGINX
                            │
                            │ FastCGI
                            ▼
                     PHP-FPM / WordPress
                            │
                            │ SQL
                            ▼
                         MariaDB
                            │
                            ▼
                     Persistent Volume
```
And the response travels back in the opposite direction:
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
The important thing to understand is not just which containers exist, but why they exist and how they communicate.

That architecture will become the foundation for the Docker Compose configuration we build next.
