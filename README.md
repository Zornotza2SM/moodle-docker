# Entorno Docker para Moodle

Este proyecto proporciona un entorno de desarrollo local basado en Docker para Moodle. Incluye Moodle, una base de datos MariaDB y un servidor Redis para el almacenamiento en caché.

## Prerrequisitos

Asegúrate de tener instalados los siguientes programas en tu sistema:

*   [Docker](https://www.docker.com/get-started)
*   [Docker Compose](https://docs.docker.com/compose/install/)

## Cómo empezar

1.  **Clona el repositorio:**

    ```bash
    git clone <URL_DEL_REPOSITORIO>
    cd B1-moodle-docker
    ```

2.  **Levanta los contenedores:**

    Ejecuta el siguiente comando para construir y levantar los contenedores en segundo plano:

    ```bash
    docker-compose up -d
    ```

    Si estás utilizando **Windows**, es posible que desees utilizar el archivo de configuración específico para Windows que optimiza el rendimiento de la base de datos:

    ```bash
    docker-compose -f docker-compose.yml -f docker-compose.windows.yml up -d
    ```

3.  **Accede a Moodle:**

    Una vez que los contenedores estén en funcionamiento, puedes acceder a Moodle en tu navegador web a través de la siguiente URL:

    [http://localhost:8080](http://localhost:8080)

    La instalación de Moodle se iniciará la primera vez. Sigue los pasos del instalador web.

## Configuración del entorno

*   **Aplicación Moodle (`web`):**
    *   **URL:** `http://localhost:8080`
    *   **Puerto expuesto:** `8080`
    *   **Directorio de datos (`moodledata`):** Los datos se guardan en el directorio local `./.data/web`.

*   **Base de datos (`db`):**
    *   **Imagen:** `mariadb:10.4`
    *   **Puerto expuesto:** `3306`
    *   **Credenciales:**
        *   **Usuario:** `moodle`
        *   **Contraseña:** `password`
        *   **Base de datos:** `moodle`
        *   **Contraseña de root:** `password`
    *   **Directorio de datos:** Los datos se guardan en el directorio local `./.data/db`.

*   **Redis (`redis`):**
    *   **Imagen:** `redis:5.0-alpine`
    *   **Puerto expuesto:** `6379`
    *   **Directorio de datos:** Los datos se guardan en el directorio local `./.data/redis`.

## Problemas Conocidos

*   Al usar un volumen compartido NFS para `/moodledata`, puedes obtener el error `session data file is not created by your uid`. Esto se debe a que el mapeo de ID de usuario del montaje NFS no es consistente con el ID de usuario local. Usa la sesión de Redis como solución alternativa.
*   Por defecto, la caché de la aplicación utiliza el almacenamiento de archivos, lo cual es bastante lento. Para mejorar el rendimiento, se recomienda configurar Redis para el almacenamiento en caché de la aplicación. Puedes hacerlo desde el menú de Moodle: `Administración del sitio` -> `Extensiones` -> `Caché` -> `Configuración` -> en la fila de `Redis`, haz clic en `Añadir instancia`.

## Error en la inicialización de la BBDD

Si sale el siguiente error, sencillamente arrancar de nuevo el contendor de la aplicación web y debería instalarse bien.
![alt text](image.png)