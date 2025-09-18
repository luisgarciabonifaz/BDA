<div class="titulo">
   Construcción de Pipelines con Apache NiFi
</div>
#  

En el mundo del Big Data, los datos rara vez se encuentran en el formato o lugar correctos para su análisis. Moverlos, limpiarlos y transformarlos es una tarea esencial, y se conoce como la creación de **pipelines de datos**. Para ello uilizaremos la herramienta NiFi

<figure markdown="span">
  ![Imagen de data pipeline](https://encrypted-tbn1.gstatic.com/licensed-image?q=tbn:ANd9GcQzBcaDNkqrhohwuMXxy21wBFngW9qPpYNYsNxQVio2g7qQah31FQr9Ok6Z3QgpIunHFArdqpf6CVdkgxHAn1wcTKe1wONup4sqQlryQpjkXrnnkts)
  <figcaption>Con licencia de Google</figcaption>
</figure>

## 1. Introducción al proceso ETL
El proceso ETL es fundamental en los sistemas de gestión de datos y en los proyectos de Business Intelligence (BI). Se utiliza para integrar datos de diferentes fuentes en un solo lugar, permitiendo un análisis más efectivo y mejorando la toma de decisiones.

- **Extract (Extracción)**: Se obtienen los datos de las fuentes originales.
- **Transform (Transformación)**: Los datos se limpian, validan y adaptan para el formato o la estructura deseada.
- **Load (Carga)**: Los datos ya transformados se cargan en el destino final (normalmente un almacén de datos o una base de datos).

## 2. Fases del proceso ETL
El proceso ETL consta de tres fases principales que trabajan en conjunto para mover, procesar y almacenar los datos desde diversas fuentes a un destino centralizado (generalmente un almacén de datos o data warehouse). 

Estos tres pasos básicos se pueden ampliar a 6 pasos cruciales que juntos componen la arquitectura ETL.

- Profiling
- Extraction
- Cleaning
- Transformation
- Loading
- Monitoring

### 2.1. Data Profiling
Data Profiling es un paso crucial, aunque a menudo ignorado, en el proceso de ETL. Consiste en examinar sus datos sin procesar y garantizar que sean apropiados para ETL.

Los tres componentes del data profiling son:

- **Descubrimiento de estructuras**: ¿Cuáles son los valores o rangos de valores aceptables para cada campo en el registro de datos?
- **Descubrimiento de contenido**: 
    - ¿Qué información contiene cada registro de datos y cuál es la calidad de este registro?
    - ¿Cómo se representan los datos que faltan (por ejemplo, con valores como NULL, 0 o -1)?
- **Descubrimiento de relaciones**:
    - ¿Cuáles son las relaciones que existen entre las fuentes de datos dadas y entre registros individuales dentro de una fuente de datos? 
    - ¿Cómo puede aprovechar estas conexiones durante ETL mientras reduce las ineficiencias (por ejemplo, eliminando datos duplicados del conjunto de datos)?

??? "Código en Python para Data Profiling"

    Probablemente haya que instalar la librería pandas_profiling con:

    ```bash
    pip install sweetviz
    ```

    Y luego crear un fichero de Python con este código:

    ```python
    import pandas as pd
    import sweetviz as sv

    # Solicitar al usuario el nombre del archivo
    file_path = input("Introduce el nombre del archivo con extensión (.csv o .xls): ")

    # Leer datos desde el archivo proporcionado por el usuario
    if file_path.endswith('.csv'):
        # Preguntar al usuario cuál es el separador del archivo CSV
        separador = input("Introduce el separador del archivo CSV (por ejemplo, ',' o ';'): ")
        df = pd.read_csv(file_path, sep=separador, encoding='unicode_escape')
    elif file_path.endswith('.xls') or file_path.endswith('.xlsx'):
        df = pd.read_excel(file_path)
    else:
        raise ValueError("Formato de archivo no compatible. Utiliza archivos .csv, .xls o .xlsx.")

    # Crear el perfil de análisis exploratorio de datos (EDA)
    report = sv.analyze(df)

    # Generar el informe y guardarlo como archivo HTML
    report.show_html('informe_eda.html')
    ```

### 2.2. Extracción (Extract)

La extracción es el primer paso del proceso ETL y consiste en obtener los datos de las diferentes fuentes de datos. 

Estas fuentes pueden ser heterogéneas y estar formadas por bases de datos, archivos de texto, aplicaciones web, logs, entre otros. 

El objetivo es recoger los datos de forma eficiente y con el menor impacto posible en los sistemas fuente.

**Tipos de extracción**:
 
 - **Extracción completa**: Se extraen todos los datos de la fuente, independientemente de si han cambiado o no. Se suele usar cuando la cantidad de datos es manejable o cuando se hace una primera carga masiva.
 - **Extracción incremental**: Solo se extraen los datos que han cambiado o que son nuevos desde la última extracción. Esto se basa en fechas de modificación o identificadores de cambios (como campos de timestamp o triggers). Este método es más eficiente para grandes volúmenes de datos.

**Desafíos de la extracción**:

- **Diversidad de las fuentes**: Puede haber múltiples fuentes con formatos y tecnologías diferentes (bases de datos SQL, NoSQL, APIs, servicios web, archivos planos, etc.).
- **Acceso a los datos**: A veces, obtener acceso a las fuentes puede ser complicado, ya sea por restricciones de seguridad, permisos o políticas de la organización.
- **Velocidad y sincronización**: Dependiendo del tipo de fuente, el acceso puede ser lento o poco sincronizado, lo que puede impactar en la puntualidad de los datos extraídos.

**Métodos de extracción**:

- **Conexión directa a la base de datos**: Utilizar SQL o consultas específicas para obtener datos directamente desde bases de datos relacionales.
- **Lectura de archivos planos**: Extraer datos de archivos CSV, JSON, XML, etc.
- **Conexión a APIs**: Obtener datos a través de solicitudes HTTP desde aplicaciones web o servicios de terceros.
- **Lectura de logs o archivos de sistema**: Procesar archivos de registros o eventos para extraer datos específicos.

**Ejemplo**:

Imagina que extraemos datos de un sistema de ventas. Podríamos conectarnos a una base de datos de MySQL para obtener registros de ventas diarias o extraer datos de un archivo JSON con detalles de productos.

### 2.3. Transformación (Transform)
La transformación es el núcleo del proceso ETL, en el cual los datos extraídos son **modificados, limpiados, integrados y estructurados** para ser consistentes, precisos y adecuados para el análisis. Aquí se aplican una serie de reglas y operaciones para preparar los datos antes de cargarlos en el destino.

**Operaciones comunes en la transformación:**

- **Limpieza de datos**: Este paso es crucial para eliminar errores, duplicados y valores faltantes. Por ejemplo:
    - Eliminar registros duplicados.
    - Gestionar valores nulos o incorrectos.
    - Validar que los datos cumplen con ciertos estándares (por ejemplo, que una fecha sea válida).
- **Normalización y estandarización**: Se ajustan los datos a un formato o estructura uniforme. Por ejemplo:
    - Convertir todos los campos de fecha a un formato común (por ejemplo, DD-MM-YYYY).
    - Transformar unidades (por ejemplo, de metros a kilómetros).
- **Conversión de tipos de datos**: Cambiar el tipo de datos de ciertos campos para asegurar la consistencia. Por ejemplo, convertir una cadena de texto que representa un número ("100") a un número entero (100).
- **Enriquecimiento de datos**: Agregar más información a los datos a partir de otras fuentes. Por ejemplo, agregar el nombre completo de un cliente basándose en su ID o agregar coordenadas geográficas en función de la dirección.
- **Agregación**: Calcular estadísticas o consolidar datos a niveles más altos. Por ejemplo, sumar las ventas diarias para obtener ventas mensuales o calcular promedios.
- **Filtrado**: Excluir datos irrelevantes o que no cumplen con ciertos criterios. Por ejemplo, descartar ventas de productos descontinuados.

**Transformación avanzada**:

- **Unión y fusión de datos**: A veces es necesario combinar datos de varias fuentes. Por ejemplo, unir los datos de clientes de una base de datos con los datos de compras en otra.
- **Data Mashing**: Combinar diferentes conjuntos de datos, incluso si provienen de fuentes externas (como APIs públicas), para generar nuevas perspectivas o análisis.
- **Transformación de claves**: Reemplazar identificadores (IDs) de bases de datos locales con identificadores globales o claves que permitan relacionar mejor los datos.

**Ejemplo**:

Imaginemos que hemos extraído un listado de ventas con fechas en diferentes formatos (DD/MM/YYYY, YYYY-MM-DD). En la fase de transformación, convertiríamos todas esas fechas a un formato uniforme, por ejemplo, YYYY-MM-DD, además de eliminar registros duplicados de ventas procesadas dos veces.

### 2.4. Carga (Load)

La carga es la fase final del proceso ETL, donde los datos transformados son almacenados en el destino final. El destino suele ser un almacén de datos o una base de datos centralizada, desde donde los datos estarán disponibles para ser consultados y analizados.

**Tipos de carga**:

- **Carga completa**: Todos los datos transformados se cargan cada vez, sobrescribiendo los datos anteriores. Este método es común cuando los volúmenes de datos son pequeños o cuando es necesario una actualización completa.
- **Carga incremental**: Solo se cargan los datos que han cambiado o que son nuevos desde la última vez que se realizó la carga. Esto es más eficiente y es el método preferido en la mayoría de los casos, ya que reduce el tiempo de procesamiento y uso de recursos.
- **Carga en tiempo real**: Los datos se cargan en el destino tan pronto como están disponibles, permitiendo análisis en tiempo real. Este método es crucial en situaciones donde es necesario procesar datos con baja latencia, como en el comercio electrónico o sistemas de seguimiento en vivo.

**Métodos de carga**:

- Insertar nuevas filas: Solo se añaden nuevos registros al destino.
- Actualizar registros existentes: Si un registro ya existe, se actualiza con la nueva información.
- Eliminar registros: En algunos casos, es necesario eliminar datos antiguos o incorrectos del destino.

**Desafíos en la carga**:

- Integridad de los datos: Asegurarse de que los datos se carguen correctamente sin perder integridad o coherencia.
- Conflictos de claves: Cuando se cargan datos que ya existen, puede haber conflictos en las claves primarias que necesiten resolverse.
- Rendimiento: La carga de grandes volúmenes de datos puede ser costosa en términos de tiempo y recursos, por lo que es importante optimizar el proceso.

**Ejemplo**:

Tras extraer y transformar datos de ventas, finalmente se cargan en un almacén de datos donde se pueden generar informes y análisis, como identificar tendencias de ventas o evaluar el rendimiento de productos.

### 2.5. Verificación y gestión de errores

Durante todos los procesos de la ETL se recomienda ir realizando comprobaciones sobre el paso anterior, por ejemplo:

- Sobre los datos en bruto verificar después de la limpieza de datos que no existen duplicados.
- Sobre una transformación o un mapeo verificar que el número de registros sigue siendo el mismo antes y después de la transformación.
- Sobre una generación de id o información agrupada verificar que no estamos perdiendo información o tenemos campos a null.

Si una ejecución de la ETL o un proceso falla…

- Logar o insertar en BD los pasos que realizamos a modo de auditoría. Añadir temporalidad.
- Si los datos se guardan en particiones, borrar la partición insertada y volver a ejecutar la ETL.
- Si es una actualización completa o una carga completa no tendremos mayor problema que volver a ejecutarlo.
- ...

El desarrollo de una ETL puede que no se acabe una vez implementada según los requisitos iniciales, siempre estará expuesta a posibles nuevos desarrollos que el cliente quiera que se implementen para poder explotar otra información o explotarla de otra forma.

Las ETL requieren de una vigilancia en cada ejecución para verificar que durante todos los procesos no han ocurrido fallos, como por ejemplo: fallos de conexiones, caída de hosts, errores en transformaciones, bugs…

### 2.6. Herramientas ETL populares
Existen múltiples herramientas que facilitan la implementación y automatización de procesos ETL. Cada una tiene características específicas, y la elección depende de las necesidades del proyecto, el presupuesto y las plataformas que se utilicen. Aquí tienes una lista de las herramientas ETL más comunes:

- **Talend.** Es una plataforma potente y flexible que soporta múltiples fuentes de datos y permite diseñar flujos ETL mediante una interfaz gráfica amigable. Tiene conectores para diversas fuentes de datos como bases de datos relacionales, NoSQL, servicios en la nube y APIs. Su versión de código abierto es ideal para pequeños proyectos, mientras que la versión comercial ofrece soporte avanzado y escalabilidad.
- **Pentaho Data Integration (PDI).** Es una herramienta ETL que permite crear flujos de datos mediante una interfaz gráfica. Es conocida por ser fácil de usar y por su capacidad para integrarse con grandes volúmenes de datos. Incluye características como la integración de Big Data, procesamiento en paralelo y opciones de integración en tiempo real.
- **Informatica PowerCenter.** Informatica PowerCenter es una de las herramientas ETL más robustas y completas para proyectos empresariales grandes. Permite el procesamiento de datos de alta calidad con un rendimiento muy elevado. Es altamente escalable y ofrece características avanzadas como gobernanza de datos, seguridad y soporte para Big Data. Sin embargo, es una solución costosa y compleja de implementar.
- **Microsoft SQL Server Integration Services (SSIS).** Es una herramienta ETL nativa de Microsoft que forma parte de SQL Server. Está completamente integrada en el ecosistema Microsoft y es ideal para empresas que ya utilizan esta plataforma. Soporta procesamiento por lotes, transformación de datos avanzada y ejecución de paquetes programados.
- **Apache NiFi.** Es una herramienta para la automatización del flujo de datos, orientada al procesamiento de datos en tiempo real. Permite mover, transformar y gestionar datos de diversas fuentes mediante un sistema gráfico basado en flujos de trabajo. Es especialmente útil para grandes volúmenes de datos en tiempo real y para la integración de IoT y Big Data.

### 2.7. ETL vs ELT

Una variante moderna de ETL es ELT (Extract, Load, Transform). La diferencia radica en el orden de las operaciones:

- En ETL, la transformación ocurre antes de cargar los datos en el destino.
- En ELT, los datos primero se cargan en el almacén de datos y luego se transforman.
- ELT es común cuando se utilizan almacenes de datos modernos o Data Lakes, ya que estos sistemas tienen la capacidad de manejar grandes volúmenes de datos sin necesidad de transformarlos antes.

### 2.8. ETL en tiempo real vs ETL por lotes

- ETL por lotes: el proceso ETL se ejecuta a intervalos regulares, procesando grandes lotes de datos de una vez. Esto es útil cuando no es necesario un procesamiento inmediato.
- ETL en tiempo real: los datos se extraen, transforman y cargan tan pronto como están disponibles. Esto es necesario en escenarios donde el análisis de datos inmediato es crucial, como en aplicaciones financieras o de monitorización.


## 3. Introducción a Apache NiFi

NiFi es una plataforma visual para el **flujo de datos** que te permite construir pipelines (ETL/ELT) complejos arrastrando y soltando componentes. Su interfaz intuitiva y su capacidad para operar a gran escala lo convierten en la herramienta perfecta para la **ingesta de datos** de diversas fuentes y su preparación para el análisis. Aquí, lo usaremos para orquestar los datos de nuestros dos proyectos, desde su origen hasta su destino final.

### 3.2 Arquitectura y Conceptos Clave

- **FlowFile:** Es el objeto de datos fundamental en NiFi. Cada pedazo de datos que fluye a través de un pipeline está encapsulado en un `FlowFile`. Contiene el contenido de los datos (el `payload`) y los **atributos** (metadatos sobre el dato, como el nombre del archivo, su origen o la fecha de creación).
- **Procesador (Processor):** Es el corazón funcional de NiFi. Un procesador es un componente que realiza una tarea específica en un `FlowFile`, como leer de un archivo, transformar datos JSON, o escribir en una base de datos.
- **Conexión (Connection):** Son las colas que conectan un procesador con otro. Almacenan los `FlowFiles` de forma segura hasta que el siguiente procesador está listo para procesarlos. Esto permite que el flujo sea asíncrono y tolerante a fallos.
- **Grupo de Procesos (Process Group):** Una forma de encapsular y organizar una colección de procesadores, conexiones y otros grupos de procesos. Sirve para crear flujos más limpios y modulares.

### 3.3. La Interfaz Gráfica de NiFi

La principal fortaleza de NiFi es su interfaz de usuario basada en web. Permite a los ingenieros de datos diseñar y monitorear pipelines de manera visual, arrastrando y soltando componentes, conectándolos y configurando sus propiedades.

- **Lienzo:** El área de trabajo principal donde se construyen los flujos.
- **Paleta de Componentes:** Panel lateral donde se encuentran todos los componentes disponibles (procesadores, conexiones, etc.).
- **Configuración:** Al hacer clic derecho en un componente, se accede a su configuración para ajustar propiedades, programar la ejecución o definir conexiones.

### 3.4. Procesadores Esenciales de NiFi

NiFi incluye cientos de procesadores listos para usar, cubriendo casi cualquier caso de uso de integración de datos. Aquí, nos centraremos en los más habituales.

**Ingesta de Datos**

- **`GetFile`:** Lee archivos de un directorio del sistema de archivos local y los convierte en `FlowFiles`.
- **`ListenHTTP`:** Inicia un servidor HTTP que escucha peticiones entrantes. Ideal para recibir datos en tiempo real de APIs o plataformas como FIWARE.
- **`GetSFTP`:** Recupera archivos de un servidor SFTP.

**Transformación de Datos**

- **`JoltTransformJSON`:** Un procesador extremadamente potente para transformar estructuras de datos JSON a otras estructuras JSON. Usaremos Jolt para extraer la información clave de las notificaciones de FIWARE.
- **`ReplaceText`:** Busca y reemplaza texto en el contenido de un `FlowFile` usando expresiones regulares.
- **`UpdateAttribute`:** Añade, actualiza o elimina atributos de un `FlowFile`. Permite enriquecer los metadatos de los datos.

**Enrutamiento y Control de Flujo**

- **`RouteOnAttribute`:** Enruta los `FlowFiles` a diferentes conexiones basándose en el valor de sus atributos. Esencial para crear lógica condicional en el flujo.
- **`RouteOnContent`:** Enruta `FlowFiles` en función del contenido de los datos.

**Carga de Datos**

- **`PutFile`:** Escribe el contenido de un `FlowFile` en un archivo en el sistema de archivos local.
- **`PutDatabaseRecord`:** Escribe registros de un `FlowFile` en una base de datos relacional.


### 3.5. Data Provenance: Trazabilidad Total del Dato

Una característica fundamental de NiFi es su capacidad para registrar el **linaje** completo de cada `FlowFile`. El módulo de **Data Provenance** de NiFi guarda un registro detallado de cada evento que ocurre sobre un `FlowFile`, como su creación, transformación, enrutamiento o eliminación.

Esto es crucial para:

- **Depuración:** Si un dato se pierde o un pipeline falla, `Data Provenance` te permite ver exactamente dónde y por qué.
- **Auditoría:** Muestra un historial completo y verificable de cómo los datos se han movido y transformado, lo cual es vital en entornos regulados.

## 4. Ejemplo Sencillo. Procesamiento de Archivos CSV

Este ejemplo simula un caso de uso común: leer un archivo de datos, procesarlo y cargarlo en un destino. El flujo hará lo siguiente:

1. **Leer** un archivo CSV del sistema de archivos local.
2. **Filtrar** los datos, quedándose solo con las filas que cumplen una condición.
3. **Convertir** el formato a JSON.
4. **Guardar** el archivo resultante en otro directorio.

Este es un ejemplo simple de un proceso **ETL** (Extract, Transform, Load).

**Paso 1: Configuración del entorno**

Antes de empezar, necesitamos dos carpetas en tu sistema:

- `input_nifi`: Aquí pondrás el archivo CSV que procesaremos.
- `output_nifi`: Aquí se guardará el resultado.

Asegúrate de que Apache NiFi tenga permisos de lectura y escritura en estas carpetas.

Archivo de entrada: sensores_ejemplo.csv

Crea un archivo llamado sensores_ejemplo.csv dentro de la carpeta input_nifi con este contenido:

``` csv
id,tipo_sensor,valor,ubicacion
1,temperatura,25.5,sala_servidores
2,humedad,60.2,sala_servidores
3,temperatura,28.1,oficina_principal
4,co2,450,oficina_principal
5,temperatura,19.0,sala_reuniones
```

**Paso 2: Construcción del Flujo en la interfaz de NiFi**

1. **Añadir el procesador `GetFile` (Extraer - E):**
    - Arrastra el icono de un procesador al lienzo.
    - En la ventana de selección, busca y elige `GetFile`.
    - Haz doble clic en el procesador `GetFile` para configurarlo.
    - En la pestaña **PROPERTIES**, establece:
        - `Input Directory`: La ruta a tu carpeta `input_nifi`.
        - `Keep Source File`: `false` (para que el archivo original se elimine después de procesarse y no se genere un bucle).
    - Haz clic en **APPLY**.
2. **Añadir el procesador `SplitText` (Pre-transformación):**
    - Este procesador no está en el temario, pero es muy útil aquí. Separará el archivo CSV grande en `FlowFiles` individuales, uno por cada línea.
    - Añade el procesador `SplitText` y conecta la relación **`success`** de `GetFile` a `SplitText`.
    - Configúralo para que divida el archivo por el número de líneas, en este caso, **`1`** (para que cada línea sea un `FlowFile`).
3. **Añadir el procesador `RouteOnAttribute` (Enrutar - T):**
    - Este procesador es crucial para el enrutamiento. Lo usaremos para filtrar los datos.
    - Añade el procesador `RouteOnAttribute` y conéctalo a `SplitText`.
    - En la pestaña **PROPERTIES**, crea una nueva propiedad personalizada.
        - **Name:** `es_temperatura`
        - **Value:** `${csv.tipo_sensor:equals('temperatura')}`
        - Esta expresión le dice a NiFi que cree una relación de salida llamada `es_temperatura` y que el `FlowFile` pasará por ahí solo si el atributo `tipo_sensor` es igual a "temperatura". El atributo se crea automáticamente por el procesador anterior (`SplitText` y su sub-procesador `ConvertCSVToJSON`).
4. **Añadir el procesador `ConvertCSVToJSON` (Transformar - T):**
    - Antes de filtrar, debemos darle una estructura. Añade este procesador para que el `FlowFile` con el texto de la línea se convierta a un `FlowFile` con un JSON.
    - Conéctalo entre `SplitText` y `RouteOnAttribute`.
    - No necesitas configurar nada más, NiFi es bastante inteligente para inferir los tipos.
5. **Añadir el procesador `PutFile` (Cargar - L):**
    - Este es el último paso.
    - Añade un procesador `PutFile` al lienzo.
    - Conecta la relación **`es_temperatura`** del procesador `RouteOnAttribute` a `PutFile`.
    - Configura `PutFile`:
        - `Directory`: La ruta a tu carpeta `output_nifi`.
        - `Conflict Resolution Strategy`: `replace` (por si el archivo ya existe).

**Paso 3: Verificación del Flujo**

Una vez que todos los componentes están conectados, puedes **iniciar** el flujo haciendo clic en el botón de inicio (el triángulo verde) en cada procesador o en el grupo de procesos.

- Verás que el archivo `sensores_ejemplo.csv` desaparece de la carpeta `input_nifi`.
- En la carpeta `output_nifi`, aparecerán tres nuevos archivos JSON, cada uno con los datos de los sensores de temperatura. Los otros sensores (humedad y CO2) habrán sido filtrados y enviados a la relación de "fallo" de `RouteOnAttribute` (que no hemos conectado a nada).

Este ejemplo ilustra la potencia de NiFi para automatizar tareas de forma visual, asegurando que los datos solo se muevan cuando se cumplen las condiciones que estableciste.

## 5. Práctica: Construyendo los Pipelines del Proyecto

### 5.1 Practica 1: Pipeline de Datos de Sensores

Construiremos un flujo de datos en NiFi que automatizará la ingesta y el procesamiento de los datos de nuestros sensores IoT.

1. **Ingesta (`ListenHTTP`):** El flujo comenzará con un procesador `ListenHTTP` que actuará como un "escuchador". FIWARE enviará las notificaciones de los sensores a este punto final, y cada notificación se convertirá en un `FlowFile`.
2. **Extracción de Datos (`JoltTransformJSON`):** Usaremos un procesador `JoltTransformJSON` para transformar el complejo cuerpo de la notificación de FIWARE y extraer solo los valores de los sensores que nos interesan (temperatura, humedad, etc.).
3. **Cálculo de Agregados (`AttributesToJSON` y `PutDatabaseRecord`):** Agregaremos procesadores para calcular los valores diarios agregados (máximo, mínimo, promedio). Los datos se agruparán por día y se calcularán los valores.
4. **Carga en el Data Lake (`PutFile`):** El resultado del cálculo se guardará en el Data Lake, probablemente en un formato optimizado para el análisis como **Parquet**.


### 5.2. Practica 2: Pipeline de Datos Academicos Históricos.

En este pipeline, procesaremos y cargaremos los datos de un conjunto histórico para alimentar el **Data Warehouse** con los datos académicos.

1. **Lectura de Datos (`GetFile`):** El flujo se iniciará con un procesador `GetFile` que leerá los ficheros CSV limpios de nuestro sistema de archivos.
2. **Uniones y Transformaciones:** Usaremos procesadores como `ReplaceText` y `RouteOnAttribute` para limpiar y transformar los datos. Luego, se usarán procesadores para realizar **uniones (joins)** de tablas y construir las **tablas de hechos y dimensiones** de nuestro modelo de datos.
3. **Carga en el Data Warehouse (`PutDatabaseRecord`):** Finalmente, los datos procesados se cargarán en las tablas finales del Data Warehouse, dejándolos listos para su análisis y visualización en el módulo de BI.