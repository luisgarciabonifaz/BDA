###  **Módulo 5: Orquestación de Datos y Construcción de Pipelines con Apache NiFi**





### **Introducción: El Corazón de la Ingeniería de Datos**



En el mundo del Big Data, los datos rara vez se encuentran en el formato o lugar correctos para su análisis. Moverlos, limpiarlos y transformarlos es una tarea esencial, y se conoce como la creación de **pipelines de datos**. Este módulo se centra en una de las herramientas más poderosas y flexibles para esta labor: **Apache NiFi**.

![Imagen de data pipeline](https://encrypted-tbn1.gstatic.com/licensed-image?q=tbn:ANd9GcQzBcaDNkqrhohwuMXxy21wBFngW9qPpYNYsNxQVio2g7qQah31FQr9Ok6Z3QgpIunHFArdqpf6CVdkgxHAn1wcTKe1wONup4sqQlryQpjkXrnnkts)

Con licencia de Google



NiFi es una plataforma visual para el **flujo de datos** que te permite construir pipelines (ETL/ELT) complejos arrastrando y soltando componentes. Su interfaz intuitiva y su capacidad para operar a gran escala lo convierten en la herramienta perfecta para la **ingesta de datos** de diversas fuentes y su preparación para el análisis. Aquí, lo usaremos para orquestar los datos de nuestros dos proyectos, desde su origen hasta su destino final.

------



### **5.1. Introducción a Apache NiFi**



**Apache NiFi** es una plataforma de software de código abierto diseñada para automatizar el flujo de datos entre sistemas de software. Su diseño se basa en la idea de que los datos son el "primer ciudadano" del sistema.



#### **Arquitectura y Conceptos Clave**



- **FlowFile:** Es el objeto de datos fundamental en NiFi. Cada pedazo de datos que fluye a través de un pipeline está encapsulado en un `FlowFile`. Contiene el contenido de los datos (el `payload`) y los **atributos** (metadatos sobre el dato, como el nombre del archivo, su origen o la fecha de creación).
- **Procesador (Processor):** Es el corazón funcional de NiFi. Un procesador es un componente que realiza una tarea específica en un `FlowFile`, como leer de un archivo, transformar datos JSON, o escribir en una base de datos.
- **Conexión (Connection):** Son las colas que conectan un procesador con otro. Almacenan los `FlowFiles` de forma segura hasta que el siguiente procesador está listo para procesarlos. Esto permite que el flujo sea asíncrono y tolerante a fallos.
- **Grupo de Procesos (Process Group):** Una forma de encapsular y organizar una colección de procesadores, conexiones y otros grupos de procesos. Sirve para crear flujos más limpios y modulares.



#### **La Interfaz Gráfica de NiFi**



La principal fortaleza de NiFi es su interfaz de usuario basada en web. Permite a los ingenieros de datos diseñar y monitorear pipelines de manera visual, arrastrando y soltando componentes, conectándolos y configurando sus propiedades.

- **Lienzo:** El área de trabajo principal donde se construyen los flujos.
- **Paleta de Componentes:** Panel lateral donde se encuentran todos los componentes disponibles (procesadores, conexiones, etc.).
- **Configuración:** Al hacer clic derecho en un componente, se accede a su configuración para ajustar propiedades, programar la ejecución o definir conexiones.

------



### **5.2. Procesadores Esenciales de NiFi**



NiFi incluye cientos de procesadores listos para usar, cubriendo casi cualquier caso de uso de integración de datos. Aquí, nos centraremos en los que usaremos en las prácticas.



#### **Ingesta de Datos**



- **`GetFile`:** Lee archivos de un directorio del sistema de archivos local y los convierte en `FlowFiles`.
- **`ListenHTTP`:** Inicia un servidor HTTP que escucha peticiones entrantes. Ideal para recibir datos en tiempo real de APIs o plataformas como FIWARE.
- **`GetSFTP`:** Recupera archivos de un servidor SFTP.



#### **Transformación de Datos**



- **`JoltTransformJSON`:** Un procesador extremadamente potente para transformar estructuras de datos JSON a otras estructuras JSON. Usaremos Jolt para extraer la información clave de las notificaciones de FIWARE.
- **`ReplaceText`:** Busca y reemplaza texto en el contenido de un `FlowFile` usando expresiones regulares.
- **`UpdateAttribute`:** Añade, actualiza o elimina atributos de un `FlowFile`. Permite enriquecer los metadatos de los datos.



#### **Enrutamiento y Control de Flujo**



- **`RouteOnAttribute`:** Enruta los `FlowFiles` a diferentes conexiones basándose en el valor de sus atributos. Esencial para crear lógica condicional en el flujo.
- **`RouteOnContent`:** Enruta `FlowFiles` en función del contenido de los datos.



#### **Carga de Datos**



- **`PutFile`:** Escribe el contenido de un `FlowFile` en un archivo en el sistema de archivos local.
- **`PutDatabaseRecord`:** Escribe registros de un `FlowFile` en una base de datos relacional.

------



### **5.3. Data Provenance: Trazabilidad Total del Dato**



Una característica fundamental de NiFi es su capacidad para registrar el **linaje** completo de cada `FlowFile`. El módulo de **Data Provenance** de NiFi guarda un registro detallado de cada evento que ocurre sobre un `FlowFile`, como su creación, transformación, enrutamiento o eliminación.

Esto es crucial para:

- **Depuración:** Si un dato se pierde o un pipeline falla, `Data Provenance` te permite ver exactamente dónde y por qué.
- **Auditoría:** Muestra un historial completo y verificable de cómo los datos se han movido y transformado, lo cual es vital en entornos regulados.

------

### 5.4. **Ejemplo Sencillo de Flujo en NiFi: Procesamiento de Archivos CSV**



Este ejemplo simula un caso de uso común: leer un archivo de datos, procesarlo y cargarlo en un destino. El flujo hará lo siguiente:

1. **Leer** un archivo CSV del sistema de archivos local.
2. **Filtrar** los datos, quedándose solo con las filas que cumplen una condición.
3. **Convertir** el formato a JSON.
4. **Guardar** el archivo resultante en otro directorio.

Este es un ejemplo simple de un proceso **ETL** (Extract, Transform, Load).



#### **Paso 1: Configuración del entorno**



Antes de empezar, necesitamos dos carpetas en tu sistema:

- `input_nifi`: Aquí pondrás el archivo CSV que procesaremos.
- `output_nifi`: Aquí se guardará el resultado.

Asegúrate de que Apache NiFi tenga permisos de lectura y escritura en estas carpetas.

Archivo de entrada: sensores_ejemplo.csv

Crea un archivo llamado sensores_ejemplo.csv dentro de la carpeta input_nifi con este contenido:

Fragmento de código

```
id,tipo_sensor,valor,ubicacion
1,temperatura,25.5,sala_servidores
2,humedad,60.2,sala_servidores
3,temperatura,28.1,oficina_principal
4,co2,450,oficina_principal
5,temperatura,19.0,sala_reuniones
```



#### **Paso 2: Construcción del Flujo en la interfaz de NiFi**



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



#### **Paso 3: Verificación del Flujo**



Una vez que todos los componentes están conectados, puedes **iniciar** el flujo haciendo clic en el botón de inicio (el triángulo verde) en cada procesador o en el grupo de procesos.

- Verás que el archivo `sensores_ejemplo.csv` desaparece de la carpeta `input_nifi`.
- En la carpeta `output_nifi`, aparecerán tres nuevos archivos JSON, cada uno con los datos de los sensores de temperatura. Los otros sensores (humedad y CO2) habrán sido filtrados y enviados a la relación de "fallo" de `RouteOnAttribute` (que no hemos conectado a nada).

Este ejemplo ilustra la potencia de NiFi para automatizar tareas de forma visual, asegurando que los datos solo se muevan cuando se cumplen las condiciones que estableciste.

### **5.5. Práctica: Construyendo los Pipelines del Proyecto**



Estas prácticas son la culminación del curso, donde pondrás en uso todo lo aprendido.



#### **(P1): Pipeline de Datos de Sensores (Proyecto 1)**



Construiremos un flujo de datos en NiFi que automatizará la ingesta y el procesamiento de los datos de nuestros sensores IoT.

1. **Ingesta (`ListenHTTP`):** El flujo comenzará con un procesador `ListenHTTP` que actuará como un "escuchador". FIWARE enviará las notificaciones de los sensores a este punto final, y cada notificación se convertirá en un `FlowFile`.
2. **Extracción de Datos (`JoltTransformJSON`):** Usaremos un procesador `JoltTransformJSON` para transformar el complejo cuerpo de la notificación de FIWARE y extraer solo los valores de los sensores que nos interesan (temperatura, humedad, etc.).
3. **Cálculo de Agregados (`AttributesToJSON` y `PutDatabaseRecord`):** Agregaremos procesadores para calcular los valores diarios agregados (máximo, mínimo, promedio). Los datos se agruparán por día y se calcularán los valores.
4. **Carga en el Data Lake (`PutFile`):** El resultado del cálculo se guardará en el Data Lake, probablemente en un formato optimizado para el análisis como **Parquet** que usaremos en un módulo posterior.



#### **(P2): Pipeline de Datos Históricos (Proyecto 2)**



En este pipeline, procesaremos y cargaremos los datos de un conjunto histórico para alimentar un **Data Warehouse**.

1. **Lectura de Datos (`GetFile`):** El flujo se iniciará con un procesador `GetFile` que leerá los ficheros CSV limpios de nuestro sistema de archivos.
2. **Uniones y Transformaciones:** Usaremos procesadores como `ReplaceText` y `RouteOnAttribute` para limpiar y transformar los datos. Luego, se usarán procesadores para realizar **uniones (joins)** de tablas y construir las **tablas de hechos y dimensiones** de nuestro modelo de datos.
3. **Carga en el Data Warehouse (`PutDatabaseRecord`):** Finalmente, los datos procesados se cargarán en las tablas finales del Data Warehouse, dejándolos listos para su análisis y visualización en el módulo siguiente.