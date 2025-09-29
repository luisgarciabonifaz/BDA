# Infraestructura

Para el desarrollo de las practicas de este modulo trabajaremos con estas herramientas y servicios

- Docker: Para crear las infraestructuras de servicios
- AWS: Para el almacenamiento y las Bases de Datos en la nube
- GitHub: Para el flujo de trabajo y la documentación 

## 1. Docker

Usaremos las siguientes infraestructuras de servicios con Docker, cada una de ellas con un fichero de configuración *docker-compose.yml*:

- **SmartCity**:  FIWARE 
- **ETL**: :NiFi
- **Orquestación**: Airflow


Cada proyecto tendrá su propio directorio con un archivo docker-compose.yml:

    ├── bigdata-proyecto-smartcity/
    │   ├── docker-compose.yml  # FIWARE, CrateDB, Grafana
    │   ├── data/
    │   │   └── sensors_data.json
    │   └── src/
    │       └── simulation_script.py
    ├── bigdata-proyecto-academico/
    │   ├── docker-compose.yml  # Base de Datos (PostgreSQL/MySQL), Herramienta ETL
    │   ├── data/
    │   │   └── calificaciones.csv
    │   └── src/
    │       └── etl_script.py
    └── README.md


??? info "Ayuda Docker"

    ### 1. Conceptos Clave

    - **Imagen:** Plantilla para un contenedor (inmutable).
    - **Contenedor:** Instancia en ejecución de una imagen.
    - **Dockerfile:** Archivo de texto con las instrucciones para construir una imagen.
    - **Modo Interactivo (`-it`):** Permite interactuar con el contenedor, útil para *shells*.

    ### 2. Gestión de Imágenes (Images)

    Las imágenes son plantillas de solo lectura usadas para crear contenedores.

    | Comando                            | Descripción                                                  | Ejemplo                           |
    | ---------------------------------- | ------------------------------------------------------------ | --------------------------------- |
    | `docker pull <imagen>`             | Descarga una imagen de un registro (por defecto Docker Hub). | `docker pull ubuntu:latest`       |
    | `docker images`                    | Lista todas las imágenes locales.                            | `docker images`                   |
    | `docker rmi <id/nombre>`           | Elimina una imagen local.                                    | `docker rmi ubuntu`               |
    | `docker build -t <nombre> .`       | Construye una imagen a partir de un **Dockerfile** en el directorio actual. | `docker build -t mi-app:v1 .`     |
    | `docker push <usuario/nombre:tag>` | Sube una imagen a un registro.                               | `docker push miusuario/mi-app:v1` |

    ### 3. Gestión de Contenedores (Containers)

    Los contenedores son instancias ejecutables de las imágenes.

    | Comando                                          | Descripción                                                  | Ejemplo                                             |      |
    | ------------------------------------------------ | ------------------------------------------------------------ | --------------------------------------------------- | ---- |
    | `docker run <imagen>`                            | Crea y ejecuta un nuevo contenedor a partir de una imagen.   | `docker run nginx`                                  |      |
    | `docker run -d <imagen>`                         | Ejecuta en **modo \*detached\*** (segundo plano).            | `docker run -d redis`                               |      |
    | `docker run -it <imagen> /bin/bash`              | Ejecuta de forma **interactiva** (`-it`) y abre una *shell*. | `docker run -it ubuntu /bin/bash`                   |      |
    | `docker run -p <host_port>:<cont_port> <imagen>` | **Mapea un puerto** del host al contenedor.                  | `docker run -p 8080:80 nginx`                       |      |
    | `docker ps`                                      | Lista los contenedores **en ejecución**.                     | `docker ps`                                         |      |
    | `docker ps -a`                                   | Lista **todos** los contenedores (incluidos los detenidos).  | `docker ps -a`                                      |      |
    | `docker stop <id/nombre>`                        | Detiene un contenedor en ejecución.                          | `docker stop mi-servidor`                           |      |
    | `docker start <id/nombre>`                       | Inicia un contenedor detenido.                               | `docker start mi-servidor`                          |      |
    | `docker restart <id/nombre>`                     | Reinicia un contenedor.                                      | `docker restart mi-servidor`                        |      |
    | `docker rm <id/nombre>`                          | Elimina un contenedor detenido.                              | `docker rm mi-contenedor`                           |      |
    | `docker exec -it <id/nombre> <comando>`          | Ejecuta un comando dentro de un contenedor en ejecución.     | `docker exec -it mi-servidor ls -l`                 |      |
    | `docker logs <id/nombre>`                        | Muestra los logs (salida estándar) de un contenedor.         | `docker logs -f mi-servidor` (con `-f` para seguir) |      |


    ### 4. Limpieza y Utilidades

    Comandos para mantener el entorno de Docker limpio.

    | Comando               | Descripción                                                  | Ejemplo               |
    | --------------------- | ------------------------------------------------------------ | --------------------- |
    | `docker system prune` | Elimina datos no utilizados: contenedores detenidos, imágenes sin etiqueta, redes sin usar. | `docker system prune` |
    | `docker volume prune` | Elimina volúmenes no utilizados.                             | `docker volume prune` |
    | `docker network ls`   | Lista las redes de Docker.                                   | `docker network ls`   |
    | `docker info`         | Muestra información detallada sobre la instalación de Docker. | `docker info`         |
    | `docker login`        | Inicia sesión en un registro de Docker (ej: Docker Hub).     | `docker login`        |

    ### 5. Ejemplos

    | Acción                 | Comando                                                | Notas                                             |
    | ---------------------- | ------------------------------------------------------ | ------------------------------------------------- |
    |Arrancar Infraestructura|	docker-compose up -d	|Levanta todos los servicios en segundo plano|.
    |Ver Logs	|docker-compose logs -f	|Monitorea la actividad de todos los servicios.|
    |Detener Infraestructura|	docker-compose down	|Detiene y elimina los contenedores (conserva las imágenes).|
    |Ejecutar Script|docker exec -it <nombre_contenedor> python src/script.py	|Ejecuta tu código (simulación, ETL) dentro de un contenedor.|


## 2. Flujo de Trabajo con Git

La infraestructura de cada proyecto se gestiona mediante **Docker Compose**, asegurando que cada entorno sea aislado y reproducible.

### Estructura de Directorios (Ejemplos)

Cada proyecto reside en su propio directorio y contiene un archivo `docker-compose.yml` para definir sus servicios.

| Proyecto                      | Archivos Clave             | Servicios Típicos                                            |
| :---------------------------- | :------------------------- | :----------------------------------------------------------- |
| `bigdata-proyecto-smartcity/` | `docker-compose.yml`       | **FIWARE** (Orion Context Broker), **CrateDB**, **Grafana**  |
|                               | `data/sensors_data.json`   |                                                              |
|                               | `src/simulation_script.py` |                                                              |
| `bigdata-proyecto-academico/` | `docker-compose.yml`       | Base de Datos (**PostgreSQL** o **MySQL**), Herramienta **ETL** (Contenedor Python) |
|                               | `data/calificaciones.csv`  |                                                              |
|                               | `src/etl_script.py`        |                                                              |

### 2.1. Commits

El historial de `commits` debe ser **constante y descriptivo** para mostrar el progreso. Se recomienda seguir un esquema de mensajes claro.

| Etapa                      | Comando `git`                      | Mensaje de Commit (Ejemplo)                                  | Propósito                                     |
| :------------------------- | :--------------------------------- | :----------------------------------------------------------- | :-------------------------------------------- |
| **Infraestructura**        | `git add docker-compose.yml`       | `feat: Inicializar infraestructura Docker con Orion, CrateDB y Grafana` | Establecer el entorno de trabajo.             |
| **Desarrollo**             | `git add src/simulation_script.py` | `feat: Implementar simulación de los tres sensores (Temp, CO2, Agua) y enviar a Orion` | Implementación de nueva funcionalidad.        |
| **Documentación/Refactor** | `git add src/etl_process.py`       | `refactor: Crear proceso ETL para calcular valores Max/Min/Medios diarios y cargar en Data Warehouse` | Mejora o reestructuración de código/procesos. |

**Recordatorio:** Después de cada `commit`, ejecuta `git push origin main` para sincronizar con el repositorio remoto.

### 2.2. README.md:

El archivo `README.md` es el **informe final** y la **guía de ejecución** del proyecto. Debe estar en la raíz del repositorio.

| Sección del README.md                | Contenido Mínimo Requerido (Ej. Proyecto Académico)          |
| :----------------------------------- | :----------------------------------------------------------- |
| 1. **Definición del Proyecto**       | **Objetivo** (Ej: Consolidar históricos e indicadores de calidad) y **enfoque** (Ej: Modelo dimensional para BI). |
| 2. **Infraestructura Técnica**       | Lista de **contenedores** (PostgreSQL/MySQL, Contenedor Python para ETL). Incluir la estructura del `docker-compose.yml`. |
| 3. **Proceso ETL y Modelo de Datos** | **Diagrama o descripción** de la transformación de archivos CSV (ITACA, PAA) en tablas de Hechos y Dimensiones. Describir las **validaciones** realizadas. |
| 4. **Instrucciones de Uso**          | Pasos **exactos** para la ejecución: `git clone`, `docker-compose up -d`, cómo ejecutar el script ETL (`python src/etl_process.py`), y cómo conectar la herramienta de BI (Ej: Power BI) al Data Warehouse. |
| 5. **Resultados (Power BI)**         | **Capturas de pantalla** del Cuadro de Mando, mostrando indicadores clave (Ej: porcentaje de aprobación) y filtros aplicables (Familia, Curso, Evaluación, etc.). |


## 3. AWS

Para  las bases de datso y el almacenamiento en la nube usaremos los servicios de **AWS** en concreto los de AWS Academy

Los servicios que usremos son :

* **Bases de Datos:** Uso de servicios gestionados de bases de datos (Ej: Amazon RDS).
* **Almacenamiento:** Uso de **S3** (Simple Storage Service) para archivos estáticos o *data lakes*.

El alumno tendra en AWS Academy:

- Un curso de Cloud Foundations
- Dos Learner Lab ( Laboratorios)  
  - Uno para los proyectos del modulo 
  - Otro para los proyectos globales


