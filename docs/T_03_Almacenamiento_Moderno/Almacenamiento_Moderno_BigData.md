
# Almacenamiento Moderno: Data Lake & Data Warehouse

Si en el módulo anterior aprendimos a "refinar" nuestros datos con Python y Pandas, ahora vamos a aprender a construir el "almacén" donde guardaremos ese valioso producto final. La forma en que almacenamos los datos es una de las decisiones de arquitectura más críticas que tomaremos. Una buena estrategia de almacenamiento garantiza que nuestras consultas sean rápidas, que nuestros sistemas sean escalables y que nuestros costes se mantengan bajo control.

En este módulo, exploraremos los tres grandes paradigmas de almacenamiento: el tradicional **Data Warehouse**, el flexible **Data Lake** y la moderna arquitectura híbrida, el **Data Lakehouse**. También descubriremos por qué el formato de fichero que elegimos (como **Parquet**) puede tener un impacto drástico en el rendimiento. Finalmente, pondremos toda esta teoría en práctica diseñando el corazón de nuestro proyecto de análisis académico: un Data Warehouse con esquema en estrella.

## 1. Paradigmas de Almacenamiento: 

**¿Biblioteca, Lago o Ambos?** La elección de la arquitectura de almacenamiento depende del tipo de datos que tenemos y, sobre todo, de lo que queremos hacer con ellos.

### 1.1. El Data Warehouse (DWH): La Biblioteca Especializada

Imagina una gran biblioteca universitaria. Los libros (datos) no se aceptan de cualquier manera. Pasan por un riguroso proceso de selección, catalogación y clasificación antes de ser colocados en estanterías específicas. Todo está perfectamente ordenado y estructurado para que un investigador (un analista de negocio) pueda encontrar la información que necesita para su tesis (un informe de BI) de la forma más rápida y fiable posible.

- **Concepto Clave:** Es un repositorio central de datos **integrados, depurados y estructurados**, optimizado para el análisis y la generación de informes (Business Intelligence).
- **Schema-on-Write (Estructura al Escribir):** La característica que lo define. Antes de que un solo dato entre en el DWH, debemos haber definido una estructura rígida (tablas, columnas, tipos de datos). Si los datos no cumplen con esa estructura, son rechazados o deben ser transformados.
- **Contenido:** Solo contiene la "joya de la corona": datos limpios, transformados y agregados que han sido validados. Es la **fuente única de la verdad (Single Source of Truth)** para la toma de decisiones estratégicas.
- **Uso Principal:** Consultas SQL complejas, informes de BI (como los que haremos con Power BI) y cuadros de mando para la dirección.
- **Metodologías de Diseño:**
  - **Inmon (Top-down):** Propone construir un gran DWH corporativo centralizado y, a partir de él, crear pequeños subconjuntos de datos llamados *Data Marts* para cada departamento.
  - **Kimball (Bottom-up):** Propone un enfoque más ágil. Se construyen primero los *Data Marts* enfocados en procesos de negocio específicos (ventas, finanzas, etc.) y luego se unen para formar un DWH consolidado. **Para nuestro Proyecto 2, seguiremos la filosofía de Kimball**, ya que diseñaremos un Data Mart específico para el análisis de resultados académicos.



### 1.2. El Data Lake: El Lago Natural

Ahora imagina un gran lago natural. Varios ríos (fuentes de datos) desembocan en él, arrastrando todo tipo de elementos: agua clara, barro, troncos, peces, plantas (datos estructurados, semi-estructurados y no estructurados). El lago lo acepta y lo almacena todo en su estado original, sin filtros.

- **Concepto Clave:** Es un repositorio de almacenamiento masivo que guarda enormes cantidades de **datos en su formato nativo y crudo**.
- **Schema-on-Read (Estructura al Leer):** La filosofía opuesta al DWH. No se define ninguna estructura al guardar los datos. La responsabilidad de interpretar y dar forma a los datos recae en el analista o científico de datos en el momento en que los va a utilizar.
- **Contenido:** ¡De todo! Desde tablas de bases de datos y CSVs hasta logs de servidores, imágenes, vídeos, clics de una web o datos de sensores de IoT. Nada se descarta.
- **Uso Principal:** Exploración de datos, análisis predictivo y Machine Learning, donde los científicos de datos necesitan acceso a los datos originales y sin procesar para descubrir patrones ocultos.
- **El Gran Reto:** Si no se gestiona adecuadamente con metadatos y un catálogo, un Data Lake puede convertirse rápidamente en un **Data Swamp (Ciénaga de Datos)**: un repositorio caótico y sin documentar donde es imposible encontrar nada de valor.

### 1.3. El Data Lakehouse: Lo Mejor de Ambos Mundos

¿Y si pudiéramos tener la escala y flexibilidad del lago, pero con la fiabilidad y el orden de la biblioteca? Esa es la promesa del Data Lakehouse. Es la arquitectura que está dominando el ecosistema de datos moderno.

- **Concepto Clave:** Es una nueva arquitectura que implementa funcionalidades de gestión y estructura propias de un Data Warehouse directamente sobre el almacenamiento de bajo coste de un Data Lake.
- **Cómo Funciona:** Se basa en una capa de metadatos y un formato de tabla de código abierto (como Delta Lake, Apache Iceberg o Apache Hudi) que se coloca "encima" de los ficheros de datos (como Parquet) en el Data Lake. Esta capa añade capacidades cruciales.
- **Superpoderes que aporta:**
  - **Transacciones ACID:** Garantiza que las operaciones sobre los datos sean fiables (o se completan todas, o no se completa ninguna), evitando la corrupción de datos.
  - **Time Travel (Viaje en el Tiempo):** Permite consultar el estado de una tabla en un punto exacto del pasado, lo que es increíble para la auditoría y la depuración.
  - **Rendimiento Optimizado:** Combina el poder del almacenamiento columnar con técnicas de indexación y caching para lograr velocidades de consulta similares a las de un DWH.
  - **Unifica el BI y el Machine Learning:** Los analistas de BI pueden ejecutar sus informes sobre datos fiables y actualizados, mientras que los científicos de datos pueden trabajar con los mismos datos para entrenar sus modelos.


## 2. Formatos de Fichero Optimizados: El Secreto de la Velocidad

Guardar los datos en un CSV es fácil, pero terriblemente ineficiente para Big Data. La elección del formato de fichero es clave.

### 2.1. Almacenamiento por Filas vs. por Columnas

Esta es la diferencia fundamental.

- **Basado en Filas (CSV, Avro):** Los datos de un mismo registro se guardan juntos en el disco. Es como leer un libro línea por línea.
  - **Ideal para:** Cargas transaccionales donde necesitas escribir o leer **un registro completo** rápidamente (ej. "dame toda la información del alumno con ID=50").
- **Basado en Columnas (Parquet, ORC):** Todos los datos de una misma columna se guardan juntos en el disco. Es como leer solo el primer capítulo de todos los libros de una estantería.
  - **Ideal para:** Cargas analíticas donde tus consultas solo implican **unas pocas columnas** de una tabla con muchas (ej. "calcula la media de las notas de todos los alumnos"). El sistema solo lee los datos de la columna "nota", ignorando el resto, lo que es increíblemente rápido.



### 2.2. Apache Parquet: El Estándar de Oro

**Parquet** es un formato de fichero de código abierto, basado en columnas, que se ha convertido en el estándar de facto en el ecosistema Big Data.

- **Rendimiento Superior:** Al ser columnar, las consultas analíticas son órdenes de magnitud más rápidas.
- **Alta Compresión:** Agrupar datos del mismo tipo permite aplicar algoritmos de compresión muy eficientes, reduciendo drásticamente el espacio de almacenamiento (y por tanto, el coste).
- **Esquema Integrado:** El propio fichero Parquet almacena la información sobre la estructura de los datos (nombres de columnas, tipos de datos). Es auto-documentado.


## 3. Gobernanza y Calidad del Dato: Poniendo Orden

Tener mucha tecnología no sirve de nada si los datos son un caos.

- **Data Governance (Gobernanza de Datos):** Es el conjunto de políticas y procesos para gestionar los datos como un activo estratégico. Responde a las preguntas: ¿Quién es el dueño de cada dato? ¿Quién tiene permiso para verlo o modificarlo? ¿Cumple con las normativas de privacidad (GDPR)?
- **Data Quality (Calidad de Datos):** Se asegura de que los datos son aptos para su uso. Un dato de calidad debe ser: **Preciso, Completo, Consistente, Válido y Actualizado.**

## 3. Gobernanza y Calidad del Dato: Poniendo Orden

Tener una arquitectura de almacenamiento potente y formatos de fichero eficientes es solo la mitad de la batalla. Sin un conjunto de reglas, procesos y controles, nuestro brillante Data Lake puede convertirse rápidamente en una "ciénaga de datos" (Data Swamp): un lugar caótico, poco fiable y, en última- instancia, inútil. Esta sección trata sobre cómo pasar de la anarquía de datos a una gestión estratégica que genere confianza y valor.

### 3.1. Data Governance (Gobernanza de Datos): El Gobierno de Nuestros Datos

La gobernanza de datos es el marco general; el conjunto de políticas, roles, estándares y procesos que garantizan que los datos de una organización se gestionan como un **activo estratégico**.

**Analogía:** Piensa en el gobierno de una ciudad. El gobierno no construye cada casa, pero establece el plan urbanístico, los códigos de construcción y las normativas (quién puede construir, dónde y cómo) para asegurar que la ciudad crezca de forma ordenada, segura y funcional. De la misma manera, la gobernanza de datos establece las "leyes" para que los datos sean fiables y valiosos.

Sus pilares fundamentales son:

- **Propiedad y Responsabilidad (Ownership & Stewardship):** Asigna responsabilidades claras. ¿Quién es el "dueño" de los datos de los clientes? ¿Quién es responsable de asegurar la calidad de los datos de ventas?
- **Políticas y Estándares:** Define reglas consistentes sobre cómo nombrar, definir, almacenar y clasificar los datos (ej. "todos los ficheros de clientes deben contener una columna `id_cliente` de tipo texto").
- **Seguridad y Privacidad:** Controla quién puede acceder a qué datos y bajo qué condiciones, asegurando el cumplimiento de normativas como el GDPR.


### 3.2. Data Quality Management (Gestión de la Calidad del Dato)

La gestión de la calidad es la implementación práctica de las políticas de gobernanza para asegurar que los datos son **aptos para su uso**. No es una tarea que se hace una sola vez, sino un proceso continuo de medición, monitorización y mejora.

Las dimensiones clave de la calidad de un dato son:

- **Precisión (Accuracy):** ¿El dato refleja correctamente el objeto o evento del mundo real? *Ej: ¿La dirección de envío de un cliente en la base de datos es realmente donde vive?*
- **Completitud (Completeness):** ¿Tenemos todos los datos que necesitamos? ¿Faltan valores en campos importantes? *Ej: Un registro de producto sin precio o sin stock.*
- **Consistencia (Consistency):** ¿El mismo dato es coherente a través de diferentes sistemas? *Ej: El cliente aparece como "CIFP Mislata" en el sistema de facturación y como "Centro Integrado Público de Formación Profesional Mislata" en el CRM.*
- **Validez (Validity):** ¿El dato se ajusta a los formatos y reglas definidos? *Ej: Un campo `codigo_postal` que contiene texto en lugar de 5 dígitos numéricos.*
- **Puntualidad/Actualidad (Timeliness):** ¿El dato está disponible y actualizado cuando se necesita? *Ej: Un informe de ventas que se genera con datos de hace un mes tiene poca utilidad para tomar decisiones diarias.*

### 3.3. Componentes Clave para una Gobernanza Activa

Para que la gobernanza no sea solo un documento teórico, se apoya en herramientas prácticas que la hacen tangible y operativa.

##### **Catálogo de Datos (Data Catalog): El "Google" de tus Datos 🗺️**


Un catálogo de datos es una herramienta centralizada que crea un inventario de todos los activos de datos de una organización. No almacena los datos en sí, sino los **metadatos** (los datos sobre los datos).

- **Analogía:** No es la biblioteca (el Data Lake), sino el **fichero de fichas o el buscador online de la biblioteca**. Te permite buscar un libro (un dataset), te dice en qué estantería está (su ubicación), de qué trata (descripción de columnas), quién es el autor (el dueño del dato) y si otros lectores lo han valorado bien (su nivel de calidad o popularidad).
- **¿Qué contiene?**
  - Descripciones de tablas y columnas en lenguaje de negocio.
  - Dueños y responsables de los datos.
  - Puntuaciones de calidad.
  - Clasificación de datos (ej. "Público", "Confidencial", "Sensible").
- **Su propósito:** Fomentar el **autoservicio**. Permite que tanto analistas como usuarios de negocio puedan **descubrir, entender y confiar** en los datos disponibles sin tener que preguntar constantemente al equipo de ingeniería.

##### **Linaje de Datos (Data Lineage): El "GPS" del Dato 🧬**


El linaje de datos es la capacidad de visualizar el **ciclo de vida completo de los datos**, trazando su recorrido desde el origen hasta su destino final.

- **Analogía:** Es como el **historial de seguimiento de un paquete de Amazon**. Te muestra dónde se originó, por qué almacenes ha pasado, qué transportista lo ha movido y dónde se ha entregado.

- **Responde a preguntas críticas:**

  - **Origen:** ¿De qué sistema o fichero proviene este número en mi informe?
  - **Transformaciones:** ¿Qué cálculos o limpiezas se le han aplicado a este dato desde que se originó?
  - **Análisis de Impacto:** Si voy a cambiar o eliminar esta tabla, ¿qué informes, dashboards o modelos de Machine Learning dejarán de funcionar?
  - **Depuración de Errores:** Si un dashboard muestra un dato incorrecto, el linaje nos permite "viajar hacia atrás" para encontrar el punto exacto donde se introdujo el error.

- **Importancia:** Es fundamental para la **confianza y la auditoría**. Herramientas como **Apache NiFi**, que usaremos en el curso, tienen capacidades de linaje visual muy potentes, permitiendo ver en tiempo real la traza completa de cada dato que fluye por el sistema.

  

## 4. Práctica: Diseñando y Construyendo Nuestro Almacén

### 4.1. Práctica 1 (Proyecto 2): Diseño del Data Warehouse Académico

Vamos a aplicar la metodología de Kimball para diseñar un *Data Mart* para nuestro proyecto. Usaremos un **Esquema en Estrella**, el modelo más común y eficiente para BI.

1. **Tabla de Hechos (Fact Table):** Es la tabla central. Contiene las métricas numéricas que queremos analizar. En nuestro caso, la **calificación**. También contiene claves foráneas que la unen a las dimensiones.
2. **Tablas de Dimensiones (Dimension Tables):** Son las tablas que rodean a la de hechos. Contienen el contexto descriptivo. Responden a las preguntas "quién, qué, dónde, cuándo, por qué".

**Nuestro Diseño:**

- **`Fact_Calificaciones`** (Contiene las métricas)
  - `id_calificacion` (Clave Primaria)
  - `calificacion_numerica` (La métrica principal)
  - `id_alumno_fk` (Clave foránea a Dim_Alumnos)
  - `id_modulo_fk` (Clave foránea a Dim_Modulos)
  - `id_tiempo_fk` (Clave foránea a Dim_Tiempo)
- **`Dim_Alumnos`** (Describe al estudiante)
  - `id_alumno_pk` (Clave Primaria)
  - `nombre_completo`
  - `turno` (Diurno, Semipresencial)
  - `grupo`
- **`Dim_Modulos`** (Describe la asignatura)
  - `id_modulo_pk` (Clave Primaria)
  - `nombre_modulo`
  - `ciclo_formativo` (DAM, DAW, etc.)
  - `grado` (Medio, Superior)
  - `familia_profesional` (Informática, Sanidad, etc.)
- **`Dim_Tiempo`** (Describe cuándo ocurrió la calificación)
  - `id_tiempo_pk` (Clave Primaria)
  - `fecha_completa` (ej. 2025-03-15)
  - `año` (2025)
  - `trimestre` (Q1)
  - `mes` (Marzo)
  - `dia_semana` (Sábado)


### 4.2. Práctica 2 (Proyectos 1 y 2): De CSV a Parquet con Python y Pandas

Ahora, vamos a convertir los ficheros CSV limpios del módulo anterior al formato Parquet, mucho más eficiente.

Python

```
# Fichero: convertir_a_parquet.py

import pandas as pd

# --- PROYECTO 2: DATOS ACADÉMICOS ---
print("Procesando datos académicos...")
# Cargar el fichero CSV limpio que generamos en el Módulo 2
try:
    df_academico = pd.read_csv('datos_procesados/Calificaciones_limpio.csv', sep=';')

    # Guardar en formato Parquet
    # 'engine='pyarrow'' especifica la librería a usar (la más común)
    # 'compression='snappy'' es un algoritmo de compresión rápido y eficiente
    output_path_academico = 'datos_almacenados/calificaciones.parquet'
    df_academico.to_parquet(output_path_academico, engine='pyarrow', compression='snappy')
    
    print(f"Datos académicos guardados en formato Parquet en: {output_path_academico}")

except FileNotFoundError:
    print("No se encontró el fichero 'Calificaciones_limpio.csv'. Asegúrate de haber completado la práctica del Módulo 2.")


# --- PROYECTO 1: DATOS DE SENSORES ---
print("\nProcesando datos de sensores...")
# Cargar el fichero JSON que generamos
try:
    df_sensores = pd.read_json('datos_generados/sensor_data.json')

    # Guardar en formato Parquet
    output_path_sensores = 'datos_almacenados/sensores.parquet'
    df_sensores.to_parquet(output_path_sensores, engine='pyarrow', compression='snappy')

    print(f"Datos de sensores guardados en formato Parquet en: {output_path_sensores}")

except FileNotFoundError:
    print("No se encontró el fichero 'sensor_data.json'. Asegúrate de haber completado la práctica del Módulo 2.")
```

Este script coge nuestros datos procesados y los almacena en un formato columnar y comprimido, dejándolos listos para ser utilizados por herramientas de procesamiento a gran escala como Apache NiFi en los próximos módulos.