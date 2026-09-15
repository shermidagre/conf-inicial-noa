**Docker Compose** te permite definir y ejecutar aplicaciones compuestas por múltiples contenedores (por ejemplo, tu aplicación Python conectada a una base de datos PostgreSQL) mediante un único archivo de configuración llamado `docker-compose.yml`.

1. **Estructurar el proyecto:** Organiza tu aplicación y sus dependencias.
Crea una carpeta para el proyecto donde incluirás tu código Python (`main.py`), el archivo `requirements.txt` y el `Dockerfile` de tu aplicación.


2. **Crear el archivo docker-compose.yml:** Configura la interacción entre contenedores.
En la raíz del proyecto, crea el archivo `docker-compose.yml` para definir los servicios que trabajarán juntos:

```yaml
version: '3.8'

services:
  # Servicio de la aplicación Python
  web:
    build: .
    ports:
      - "8000:8000"
    environment:
      - DB_HOST=db
      - DB_USER=postgres
      - DB_PASSWORD=secreto
    depends_on:
      - db

  # Servicio de la base de datos PostgreSQL
  db:
    image: postgres:15-alpine
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: secreto
      POSTGRES_DB: mi_base_datos
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:

```


3. **Construir y arrancar el entorno:** Compila e inicia todos los contenedores a la vez.
Abre la terminal en la carpeta de tu proyecto y ejecuta:

```bash
docker compose up -d

```

*El parámetro `-d` (detached) ejecuta los contenedores en segundo plano para dejar la terminal libre.*


4. **Verificar el estado y los logs:** Supervisa el estado y los mensajes del sistema.
* Para verificar qué contenedores están corriendo:

```bash
docker compose ps

```

* Para ver los registros de salida (logs) en tiempo real:

```bash
docker compose logs -f

```


5. **Detener la infraestructura:** Gestiona el apagado sin perder información.
* **Detener los servicios sin borrar datos:**

```bash
docker compose stop

```

* **Detener y eliminar contenedores y redes creadas:**

```bash
docker compose down

```


---

### Conceptos clave de `docker-compose.yml`

* **`services`:** Define cada contenedor que compone tu aplicación.
* **`build`:** Indica a Compose que debe construir la imagen usando el `Dockerfile` local.
* **`image`:** Descarga una imagen ya existente desde Docker Hub (en este caso, PostgreSQL).
* **`depends_on`:** Garantiza el orden de inicio (arranca la base de datos antes que la aplicación web).
* **`volumes`:** Mantiene la información de la base de datos guardada en tu equipo aunque se elimine el contenedor.