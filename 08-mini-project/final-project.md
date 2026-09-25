# What We Built

At this point, we have transformed the architecture into a real Docker application:
```text
                         Browser
                            │
                            │ :8080
                            ▼
                     ┌─────────────┐
                     │    NGINX    │
                     └──────┬──────┘
                            │
                         FastCGI
                            │
                            ▼
                  ┌──────────────────┐
                  │ WordPress/PHP-FPM│
                  └────────┬─────────┘
                           │
                          SQL
                           │
                           ▼
                    ┌─────────────┐
                    │   MariaDB   │
                    └──────┬──────┘
                           │
                           ▼
                    mariadb-data
```
We have now combined the concepts from the previous sections:

- Images from Dockerfiles
- Containers from those images
- Networks for service communication
- Volumes for persistent data
- Docker Compose to define and run the complete application

The next step is to run the application, inspect each container, and troubleshoot anything that doesn't work.

## AT THE END OF THIS PROJECT
I really hope you enjoyed this mini-project. You should now have a good understanding of how to build a multi-container application with Docker and Docker Compose.