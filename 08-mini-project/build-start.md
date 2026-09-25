# Build and Start the Application

From the project directory:
```
docker compose up --build
```
The --build option tells Compose to rebuild the images before starting the services.

You should see the three services start:
```text
mariadb
wordpress
nginx
```
To run them in the background instead:
```
docker compose up -d --build
```
Check their status:
```
docker compose ps
```
You should see all three containers running.