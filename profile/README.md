# Sistema de Seguimiento de Docencias - Guía del Desarrollador
Este documento es una guía para desarrolladores sobre cómo desplegar un sistema diseñado para facilitar la gestión y el seguimiento de las actividades de docencia dentro de instituciones educativas. El objetivo principal es proporcionar una herramienta que permita a los educadores y administradores organizar, registrar y monitorear el progreso académico y las responsabilidades de enseñanza de manera eficiente.
# Desarrollo
Para una guía detallada sobre el setup del entorno de desarrollo, ver el README.md de cada repositorio.
## Diagrama de tablas
![image](https://github.com/user-attachments/assets/2e0c1724-be3c-4758-9167-53274d2be552)


# Despliegue
Clona ambos repositorios, luego, en la carpeta raiz a ambas, crea un archivo docker-compose y un archivo .env

## docker-compose.yml
```yml
services:
  seguimientos-postgres-db:
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
      -  seguimientos-postgres-db
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
  seguimientos-frontend:
    build: Frontend-SistemaDeSeguimiento/frontend/.
    container_name: seguimientos-frontend
    ports:
      - "3000:3000"
    depends_on:
      - seguimientos-backend
    environment:
      ORIGIN: ${ORIGIN}
      API_URI: ${API_URI}
      BACKEND_PUBLIC_URL: ${BACKEND_PUBLIC_URL}
      NODE_ENV: ${NODE_ENV}
    env_file:
      - .env
volumes:
    postgres_data:
```
## Ejemplo de .env
```py
SECRET_KEY="django-insecure-1seo&72qbf8p6z2t&7m2%_mvnbslm$)g#wy4ix@bbruuwmug"
DEBUG=True
DB_NAME="seguimientos"
DB_USERNAME="postgres"
DB_PASSWORD="1234"
DB_HOST="seguimientos-postgres-db"
DB_PORT="5432"
DEFAULT_FROM_EMAIL="mail@mailserver.com" #Dirección desde la que se mandarán los correos
ORIGIN="http://0.0.0.0:3000"
API_URI = "http://seguimientos-backend:8000"
BACKEND_PUBLIC_URL = "http://0.0.0.0:8001"
NODE_ENV="debug"
```
Luego, ejecuta:
```bash
docker compose up
docker exec -it seguimientos-backend python manage.py migrate
docker exec -it seguimientos-backend python manage.py createsuperuser
```
