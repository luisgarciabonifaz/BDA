# BDA

## Temas


| Módulo                                                          | Horas|RA's|
| --------------------------------------------------------------- | ---- |----|
| Módulo 0: Infraestructura Big Data                              | 2    ||
| Módulo 1: Fundamentos y Ecosistema Moderno de Big Data          | 9    |RA1|
| Módulo 2: Bases de Datos NoSQL en la Práctica                   | 11   |RA2,RA3|
| Módulo 3: Almacenamiento Moderno: Data Lake & Data Warehouse    | 13   |RA2.RA3|
| Módulo 4: Procesamiento y Flujos de Datos (ETL) con Apache NiFi | 18   |+RA3|
| Módulo 5: Orquestación de Flujos                                | 20   |RA4|
| Módulo 6: Inteligencia de Negocio y Visualización con Power BI  | 16   |RA5+RA2|
| Módulo 7: Visualización de Datos en Tiempo Real y Cierre        | 11   |RA6+RA4|
| **Total**                                                       |**95**|

Exportar a Hojas de cálculo

RA3 de BDA Se elimina

**RA's SBD**
-----------
RA1 --> Spark
RA2 --> Modulo 6
RA3 --> Modulo 4
RA4 --> Modulo 7


## Descripción



## Módulo 1: Fundamentos y Ecosistema Moderno de Big Data (8 horas)
Se introduce el campo del Big Data, no solo con la teoría clásica, sino presentando desde el primer día el "Modern Data Stack" y los dos proyectos que se desarrollarán a lo largo del curso.

1.1. ¿Qué es Big Data?

Las 5 'V' (Volumen, Velocidad, Variedad, Veracidad, Valor).

El rol del Ingeniero de Datos, Analista de Datos y Científico de Datos.

1.2. El Ecosistema de Datos Moderno.

Visión general de arquitecturas Cloud (AWS, Azure, GCP).

Componentes clave: Ingesta, Almacenamiento (Data Lake, Data Warehouse), Procesamiento, Orquestación y Visualización.

Introducción a conceptos como Data Lakehouse e ELT.

1.3. Presentación de los Proyectos del Curso. 🎯

Proyecto 1 (P1): Smart City. Se presenta como un reto de datos en streaming (IoT).

Proyecto 2 (P2): Análisis Académico. Se presenta como un reto de Business Intelligence y Data Warehousing.

1.4. Práctica Inicial:

(P1): Diseño inicial de las 3 entidades (Sensor Temperatura, CO2, Calidad Agua) en un documento.

(P2): Análisis exploratorio inicial de los ficheros CSV proporcionados.

## Módulo 2: Bases de Datos NoSQL en la Práctica con FIWARE (10 horas)
Introducción al módulo:
No todos los datos encajan bien en las tablas y filas de las bases de datos tradicionales. Este módulo explora el mundo de las bases de datos NoSQL, diseñadas para la flexibilidad y la escala. Nos centraremos en un caso de uso muy práctico: la plataforma FIWARE para IoT, que utiliza internamente bases de datos NoSQL para gestionar los datos de los sensores en tiempo real de nuestro Proyecto 1.

4.1. Introducción a NoSQL: Cuándo y por qué usarlas frente a SQL.

4.2. Tipos de Bases de Datos NoSQL.

Documentales (MongoDB), Clave-Valor (Redis, DynamoDB), Columnares (Cassandra) y de Grafos (Neo4j).

4.3. FIWARE y su integración con Bases de Datos NoSQL.

El rol del Orion Context Broker como gestor de estado.

Cómo FIWARE se integra con bases de datos como MongoDB (para el estado actual) y CrateDB o TimescaleDB (para el histórico de datos).

4.4. Práctica (P1):

Configuración del entorno FIWARE (Orion Context Broker y una base de datos para persistencia).

Creación de las 3 entidades definidas en el Módulo 1 a través de la API de Orion.

Creación de una suscripción para que cualquier cambio en las entidades se guarde automáticamente en la base de datos histórica.

Cargar los 400 datos por atributo usando el script de Python del Módulo 2 para enviar actualizaciones a Orion.

## Módulo 3: Almacenamiento Moderno: Data Lake & Data Warehouse (12 horas)
Introducción al módulo:
Un proyecto de datos es tan sólido como sus cimientos, y esos cimientos son el almacenamiento. En este módulo, profundizaremos en los dos pilares del almacenamiento de Big Data: el Data Lake, para guardar datos en su formato original, y el Data Warehouse, para almacenar datos estructurados y listos para el análisis. Veremos cómo ambos conceptos pueden convivir e incluso fusionarse en la arquitectura moderna del Data Lakehouse.

3.1. Paradigmas de Almacenamiento.

Data Warehouse (DWH): Enfocado en el análisis de negocio. Estructura y propósito (modelado dimensional con esquemas en estrella/copo de nieve).

Data Lake (DL): Flexibilidad para datos crudos, semi-estructurados y no estructurados.

El Data Lakehouse: La arquitectura híbrida que combina la flexibilidad del Data Lake con las capacidades de gestión del Data Warehouse (ej. Databricks Delta Lake, Apache Iceberg).

3.2. Formatos de Fichero Optimizados para Big Data.

Parquet: El estándar de facto. Almacenamiento columnar, compresión y optimización de consultas.

Otros formatos como ORC y Avro.

3.3. Gobernanza y Calidad del Dato (Data Governance).

Conceptos de linaje de datos, catalogación y gestión de la calidad.

3.4. Práctica:

(P2): Diseñar en detalle el esquema en estrella para el Data Warehouse académico. Definir las tablas de hechos (calificaciones) y las tablas de dimensiones (alumnos, ciclos, tiempo, etc.) con sus respectivas columnas y tipos de datos.

(P1 y P2): Usar el script de Python del módulo anterior para convertir los datos generados (sensores) y los datos limpios (académicos) a formato Parquet.


## Módulo 4: Procesamiento y Flujos de Datos (ETL) con Apache NiFi (16 horas)
Introducción al módulo:
Este es el corazón práctico del curso. Aquí aprenderemos a mover, transformar y dirigir los datos de un lugar a otro usando Apache NiFi. NiFi es una herramienta visual e interactiva que permite diseñar flujos de datos (ETL) complejos de forma intuitiva, conectando diferentes sistemas y procesando la información en tiempo real o por lotes. Es la herramienta perfecta para construir los pipelines de nuestros dos proyectos.

5.1. Introducción a Apache NiFi.

Arquitectura y conceptos clave: FlowFile, Procesador, Conexión y Process Group.

La interfaz gráfica de NiFi: Construyendo flujos de datos arrastrando y soltando componentes.

5.2. Procesadores Esenciales de NiFi.

Ingesta de datos: GetFile, ListenHTTP, GetSFTP.

Transformación: JoltTransformJSON, ReplaceText, UpdateAttribute.

Enrutamiento: RouteOnAttribute, RouteOnContent.

Carga de datos: PutFile, PutDatabaseRecord.

5.3. Data Provenance: Trazabilidad total del dato.

Cómo NiFi registra cada paso que sigue un dato, garantizando el linaje y facilitando la depuración.

5.4. Práctica:

(P1): Crear un flujo en NiFi que:

Escuche las notificaciones de FIWARE (vía HTTP).

Extraiga los datos de los sensores del cuerpo de la notificación.

Calcule los agregados diarios (máximo, mínimo, media).

Guarde estos agregados en el Data Lake en formato Parquet.

(P2): Crear un flujo en NiFi que:

Lea los ficheros CSV limpios del sistema de archivos.

Realice las uniones (Join) y transformaciones necesarias para construir las tablas de hechos y dimensiones.

Cargue los datos procesados en las tablas del Data Warehouse.

## Módulo 5: Orquestación y Automatización de Flujos (10 horas)
Introducción al módulo:
Un flujo de datos no es útil si tenemos que ejecutarlo manualmente cada vez. La orquestación consiste en programar, automatizar y gestionar la ejecución de nuestros pipelines de datos. Aunque NiFi tiene sus propias capacidades de programación, lo integraremos con herramientas estándar de la industria como Apache Airflow para gestionar dependencias complejas y tener un control centralizado de todos nuestros procesos.

6.1. ¿Qué es la Orquestación de Datos?

Diferencia entre orquestación y coreografía.

6.2. Herramientas de Orquestación Basadas en Código.

Apache Airflow: El estándar de la industria. Conceptos de DAG, Operadores y Tareas.

Alternativas como Mage o Prefect.

6.3. Integración de NiFi con Orquestadores.

Cómo usar la API de NiFi para iniciar y detener flujos de datos desde una herramienta externa como Airflow.

6.4. Práctica:

(P1 y P2): Crear un DAG (Directed Acyclic Graph) simple en Airflow que se ejecute diariamente y que, a través de la API de NiFi, inicie el flujo de datos correspondiente a cada proyecto. Esto simula un entorno de producción real donde los procesos ETL están totalmente automatizados.

## Módulo 6: Inteligencia de Negocio y Visualización con Power BI (14 horas)
Introducción al módulo:
Los datos solo tienen valor si podemos convertirlos en conocimiento. Este módulo se centra en la última milla del viaje del dato: la visualización y la inteligencia de negocio (BI). Aprenderemos a usar Power BI, una de las herramientas líderes del mercado, para conectar nuestros datos procesados, modelarlos y crear informes interactivos que respondan a preguntas de negocio clave para nuestro Proyecto 2.

7.1. Fundamentos de Business Intelligence (BI).

7.2. Modelado de Datos para BI con Power BI.

Conexión a fuentes de datos (ficheros Parquet, bases de datos).

Importancia del esquema en estrella creado previamente.

Creación de relaciones, jerarquías y columnas calculadas.

Introducción a DAX (Data Analysis Expressions) para crear métricas personalizadas.

7.3. Principios de Diseño de Dashboards Efectivos.

7.4. Práctica (P2):

Conectar Power BI a las tablas del Data Warehouse generadas por NiFi.

Implementar el modelo de datos (relaciones entre tablas de hechos y dimensiones) en Power BI.

Crear el cuadro de mando de resultados académicos, incluyendo los filtros solicitados (Familia, Curso, Grado, etc.) y al menos 2 visualizaciones interactivas y 2 métricas calculadas con DAX.

## Módulo 7: Visualización de Datos en Tiempo Real y Cierre (10 horas)
Introducción al módulo:
Para datos que cambian constantemente, como los de nuestro proyecto de Smart City, necesitamos herramientas de visualización diferentes. En este módulo final, aprenderemos a usar Grafana, el estándar para la monitorización y visualización de datos de series temporales. Cerraremos el curso preparando la documentación final y presentando los dos proyectos completos, demostrando así el dominio de todo el ciclo de vida del dato.

8.1. Herramientas para Dashboards en Tiempo Real.

Grafana: El estándar para métricas y datos de series temporales (Time Series).

8.2. Conceptos Clave en Grafana.

Data Sources, Queries, Panels, Variables y Alertas.

8.3. Práctica (P1):

Conectar Grafana a la base de datos donde FIWARE almacena el histórico de los sensores.

Crear el panel de control con 4 visualizaciones (ej. un gráfico de temperatura a lo largo del tiempo, un medidor para el nivel de CO2 actual, etc.).

Implementar una variable para poder seleccionar el sensor a visualizar.

Configurar una alerta que se dispare si un valor (ej. temperatura) supera un umbral definido.

8.4. Documentación y Presentación de Proyectos.

Elaboración del documento explicativo final para ambos proyectos, detallando la arquitectura, los flujos de datos y las decisiones de diseño.

Presentación final de resultados.

