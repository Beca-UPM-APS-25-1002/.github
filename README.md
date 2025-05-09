# docker-compose.yml

```yml
services:
 seguimientos-db:
   image: postgres:17
   environment:
     POSTGRES_DB: ${DB_NAME}
     POSTGRES_USER: ${DB_USERNAME}
     POSTGRES_PASSWORD: ${DB_PASSWORD}
   ports:
     - "5432:5432"
   volumes:
     - postgres_data:/var/lib/postgresql/data
   env_file:
     - .env

 seguimientos-backend:
   build: Backend-SistemaDeSeguimiento/.
   container_name: seguimientos-backend
   ports:
     - "8000:8000"
   depends_on:
     - seguimientos-db
   environment:
     SECRET_KEY: ${SECRET_KEY}
     DEBUG: ${DEBUG}
     DB_NAME: ${DB_NAME}
     DB_USERNAME: ${DB_USERNAME}
     DB_PASSWORD: ${DB_PASSWORD}
     DB_HOST: ${DB_HOST}
     DB_PORT: ${DB_PORT}
   env_file:
     - .env
volumes:
   postgres_data:
```
