# Deploy
Clone both repositories, then in the parent folder create a docker-compose and .env
## docker-compose.yml
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
## .env example
```py
SECRET_KEY="django-insecure-1seo&72qbf8p6z2t&7m2%_mvnbslm$)g#wy4ix@bbruuwmug"
DEBUG=True # This should be false for production
DB_NAME="seguimientos"
DB_USERNAME="postgres"
DB_PASSWORD="1234"
DB_HOST="seguimientos-postgres-db" #As it is in a docker network use the Database container name
DB_PORT="5432"
```
## Then run
`docker compose up`
`docker exec -it seguimientos-backend python manage.py migrate`
`docker exec -it seguimientos-backend python manage.py createsuperuser`
