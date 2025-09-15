Aquí tienes una versión ampliada y detallada del temario de tu módulo, estructurada como apuntes de clase. El contenido está diseñado para ser directo, práctico y fácil de seguir.



### 💡 **Módulo 4: Bases de Datos NoSQL y FIWARE para IoT**



------



### **Introducción: Más allá de la Tabla Relacional**



No todos los datos encajan perfectamente en las estructuras rígidas de filas y columnas de las bases de datos relacionales (SQL) que conocemos. En el mundo del Big Data, los datos pueden ser variados (estructurados, semi-estructurados, no estructurados), masivos y generados a gran velocidad. Aquí es donde entran en juego las bases de datos **NoSQL** (Not Only SQL), una categoría de sistemas de gestión de bases de datos que ofrecen una flexibilidad, escalabilidad y rendimiento excepcionales para estos escenarios.

En este módulo, no solo exploraremos los fundamentos de NoSQL, sino que los aplicaremos a un caso de uso real y relevante: la plataforma **FIWARE** para la gestión de datos de Internet de las Cosas (IoT). Utilizaremos FIWARE para manejar los datos en tiempo real de los sensores de nuestro **Proyecto 1**, aprendiendo cómo esta plataforma se integra con distintas bases de datos NoSQL para gestionar el estado actual y el historial de los datos.

------



### **4.1. Introducción a NoSQL: Cuándo y por qué usar NoSQL frente a SQL**



Las bases de datos relacionales (SQL) son fantásticas para datos que se ajustan a un esquema fijo y tienen relaciones bien definidas (ej. un cliente tiene muchos pedidos). Sin embargo, cuando los datos son heterogéneos y cambian constantemente, como en una red de sensores IoT, las bases de datos NoSQL ofrecen una solución más ágil y eficiente.

**Ventajas de NoSQL:**

- **Flexibilidad de esquema:** No hay un esquema predefinido. Puedes almacenar diferentes tipos de datos dentro de la misma colección, lo cual es ideal para datos semi-estructurados como JSON, que es el formato principal en el IoT.
- **Escalabilidad horizontal:** Las bases de datos NoSQL están diseñadas para escalar fácilmente en clústeres de servidores de bajo coste. Esto se conoce como escalabilidad horizontal y es crucial para manejar enormes volúmenes de datos.
- **Rendimiento:** Al estar diseñadas para casos de uso específicos, muchas bases de datos NoSQL ofrecen un rendimiento superior para lecturas y escrituras intensivas.

------



### **4.2. Tipos de Bases de Datos NoSQL: Un Vistazo Rápido**



Cada tipo de base de datos NoSQL está optimizado para un propósito diferente. Entender sus características te ayudará a elegir la herramienta adecuada para cada tarea.

- **Bases de Datos de Documentos (Document Stores):**
  - **Concepto:** Almacenan datos en documentos similares a JSON (MongoDB usa BSON, una versión binaria de JSON). Cada documento puede tener un esquema diferente, lo que las hace increíblemente flexibles.
  - **Ejemplo clave:** **MongoDB**.
  - **Casos de uso:** Sistemas de gestión de contenidos, catálogos de productos y, en nuestro caso, el estado actual de las entidades de IoT.
- **Bases de Datos de Clave-Valor (Key-Value Stores):**
  - **Concepto:** El tipo más simple de NoSQL. Cada dato es un par **clave-valor**. Son extremadamente rápidas para operaciones de lectura y escritura.
  - **Ejemplos clave:** **Redis** (memoria), **DynamoDB** (Amazon).
  - **Casos de uso:** Caching de datos, gestión de sesiones de usuarios y tablas de búsqueda rápidas.
- **Bases de Datos Columnares (Wide-Column Stores):**
  - **Concepto:** En lugar de almacenar datos por filas, los almacenan por columnas. Esto las hace muy eficientes para consultas analíticas sobre grandes conjuntos de datos, ya que solo se leen las columnas necesarias.
  - **Ejemplo clave:** **Cassandra**.
  - **Casos de uso:** Analítica de Big Data, series de tiempo (Time Series) y bases de datos de eventos.
- **Bases de Datos de Grafos (Graph Databases):**
  - **Concepto:** Almacenan datos y las relaciones entre ellos en una estructura de grafo, donde los datos son **nodos** y las relaciones son **aristas**. Son ideales para analizar interconexiones complejas.
  - **Ejemplo clave:** **Neo4j**.
  - **Casos de uso:** Redes sociales, detección de fraude y sistemas de recomendación.

------

Por supuesto, aquí tienes apuntes básicos sobre **MongoDB**, la base de datos de documentos que utilizaremos en el módulo, incluyendo ejercicios prácticos.

------



### **4.3. ¿Qué es MongoDB?** 📜



MongoDB es una base de datos NoSQL de tipo **documental**. A diferencia de las bases de datos relacionales que usan tablas, MongoDB organiza los datos en **colecciones**, que son equivalentes a las tablas, y cada colección contiene **documentos**, que son equivalentes a las filas.

La clave de MongoDB es que cada documento es una estructura flexible, similar a **JSON** (JavaScript Object Notation), que puede contener campos anidados, listas y otros documentos. Esto elimina la necesidad de un esquema fijo y hace que sea ideal para datos semi-estructurados como los que provienen de sensores IoT.

**Conceptos Clave:**

- **Documento:** Un registro individual en MongoDB. Es una estructura de datos en formato **BSON** (una versión binaria de JSON), que puede tener diferentes campos y tipos de datos.
- **Colección:** Un grupo de documentos. Es el equivalente a una tabla en bases de datos relacionales. No se aplica un esquema estricto a los documentos de una colección.
- **Base de Datos:** Un contenedor físico para colecciones. Puedes tener múltiples bases de datos en una misma instancia de MongoDB.

------



### **Comandos Básicos de MongoDB (Shell)** 👩‍💻



Para interactuar con MongoDB, usaremos el **Mongo Shell**. A continuación, los comandos más comunes:

**1. Ver bases de datos y cambiar de base de datos**

- `show dbs;` : Muestra todas las bases de datos disponibles.
- `use [nombre_base_de_datos];` : Selecciona o crea una base de datos. Por ejemplo, `use fiware_db;`.

**2. Insertar Documentos**

- `db.[nombre_coleccion].insertOne({ ... });` : Inserta un único documento.
- `db.[nombre_coleccion].insertMany([{ ... }, { ... }]);` : Inserta múltiples documentos.

**3. Buscar Documentos**

- `db.[nombre_coleccion].find();` : Encuentra todos los documentos de una colección.
- `db.[nombre_coleccion].find({ [criterio] });` : Busca documentos que coincidan con un criterio específico. Por ejemplo, `db.sensores.find({ tipo: "temperatura" });`.
- `db.[nombre_coleccion].findOne({ ... });` : Devuelve el primer documento que coincide.
- `db.[nombre_coleccion].find().pretty();` : Muestra los resultados de forma legible.

**4. Actualizar Documentos**

- `db.[nombre_coleccion].updateOne({ [criterio] }, { $set: { [campo]: [valor] } });` : Actualiza un solo documento.
- `db.[nombre_coleccion].updateMany({ [criterio] }, { $set: { [campo]: [valor] } });` : Actualiza múltiples documentos.

**5. Eliminar Documentos**

- `db.[nombre_coleccion].deleteOne({ [criterio] });` : Elimina el primer documento que coincide.
- `db.[nombre_coleccion].deleteMany({ [criterio] });` : Elimina todos los documentos que coinciden.

------



### **Ejercicios Prácticos** 🏋️‍♀️



¡Manos a la obra! Imagina que tienes una base de datos llamada `iot_data` para el Proyecto 1.

**Ejercicio 1: Crear una base de datos e insertar un sensor**

1. Abre el Mongo Shell y crea una base de datos llamada `iot_data`.
2. Crea una colección llamada `dispositivos`.
3. Inserta un documento en la colección `dispositivos` con la siguiente información:
   - `id_sensor`: "temp01"
   - `tipo`: "temperatura"
   - `ubicacion`: "sala_servidores"
   - `valor_actual`: 24.5
   - `unidad`: "celsius"

JavaScript

```
use iot_data;
db.dispositivos.insertOne({
    id_sensor: "temp01",
    tipo: "temperatura",
    ubicacion: "sala_servidores",
    valor_actual: 24.5,
    unidad: "celsius"
});
```

**Ejercicio 2: Insertar múltiples sensores y buscar por tipo**

1. Inserta dos nuevos documentos en la colección `dispositivos`: uno para un sensor de humedad (`hum01`) y otro para un sensor de CO2 (`co201`), con valores y ubicaciones que elijas.
2. Busca y muestra solo los documentos de los sensores de tipo "temperatura".

JavaScript

```
// (Asume que estás en la base de datos iot_data)
db.dispositivos.insertMany([
    {
        id_sensor: "hum01",
        tipo: "humedad",
        ubicacion: "sala_servidores",
        valor_actual: 60.2,
        unidad: "porcentaje"
    },
    {
        id_sensor: "co201",
        tipo: "co2",
        ubicacion: "oficina_principal",
        valor_actual: 450,
        unidad: "ppm"
    }
]);

db.dispositivos.find({ tipo: "temperatura" }).pretty();
```

**Ejercicio 3: Actualizar el valor de un sensor**

1. Actualiza el valor actual del sensor con `id_sensor: "temp01"` a 25.8.
2. Verifica que el valor se haya actualizado correctamente buscando el documento del sensor.

JavaScript

```
db.dispositivos.updateOne(
    { id_sensor: "temp01" },
    { $set: { valor_actual: 25.8 } }
);

db.dispositivos.find({ id_sensor: "temp01" }).pretty();
```

**Ejercicio 4: Eliminar un sensor**

1. Elimina el documento del sensor de CO2 (`co201`) de la colección.
2. Verifica que el documento ya no exista.

JavaScript

```
db.dispositivos.deleteOne({ id_sensor: "co201" });
db.dispositivos.find({ id_sensor: "co201" }); // Esto no debería devolver nada
```

### **4.4. FIWARE y su Integración con Bases de Datos NoSQL**



FIWARE es un marco de software de código abierto que facilita el desarrollo de aplicaciones inteligentes. Su componente central para la gestión de datos es el **Orion Context Broker**.



#### **El Rol del Orion Context Broker como Gestor de Estado**



Imagina que el **Orion Context Broker** es el "cerebro" de nuestro sistema IoT. Su función principal es gestionar el **estado en tiempo real** de las entidades de nuestro proyecto (ej. sensores). Cuando un sensor envía datos, Orion recibe la información y la actualiza en su base de datos interna. Esta base de datos, por defecto, suele ser **MongoDB**.

**¿Por qué MongoDB?** Porque su modelo de documentos se adapta perfectamente a la estructura JSON de los datos de los sensores, permitiendo que el estado de cada entidad (sensor, dispositivo, etc.) se represente como un solo documento.



#### **Bases de Datos para el Histórico de Datos**



El Orion Context Broker solo guarda el estado **actual** de cada entidad. Para almacenar el **histórico de datos** (cada una de las 400 actualizaciones que enviaremos), necesitamos un componente adicional llamado **QuantumLeap** que se integra con bases de datos especializadas en series de tiempo. Estas bases de datos están optimizadas para almacenar y consultar datos ordenados por fecha.

- **CrateDB:** Una base de datos distribuida y rápida, ideal para series de tiempo.
- **TimescaleDB:** Una extensión de PostgreSQL que le añade capacidades de series de tiempo.

La arquitectura común en FIWARE es que el **Orion Context Broker** actualice el estado actual en una base de datos de documentos (MongoDB), mientras que una **suscripción** se encarga de enviar una copia de cada actualización a una base de datos histórica (CrateDB o TimescaleDB).

------



### **4.4. Práctica (P1): Manos a la Obra**



Esta práctica es el corazón del módulo. Vamos a construir una solución completa de gestión de datos de IoT.

1. **Configuración del entorno FIWARE:**
   - Utilizaremos **Docker Compose** para levantar los servicios necesarios:
     - `orion`: El Orion Context Broker.
     - `mongo`: La base de datos MongoDB para el estado actual.
     - `quantumleap`: El componente para el histórico.
     - `cratedb`: La base de datos para el histórico de datos.
   - Asegúrate de que todos los contenedores se comuniquen correctamente a través de la red de Docker.
2. **Creación de las 3 entidades:**
   - Usando la **API REST de Orion Context Broker**, crearemos las 3 entidades que definimos en el Módulo 1 (ej. un sensor de temperatura, un sensor de humedad, etc.). Cada entidad tendrá sus propios atributos.
   - La llamada API enviará un `POST` a `/v2/entities` con el JSON de cada entidad.
3. **Creación de una suscripción:**
   - Esta es la parte clave para el histórico. Crearemos una **suscripción** en Orion que "escucha" cualquier cambio en nuestras entidades.
   - Cuando Orion detecte una actualización, enviará automáticamente una copia de la entidad modificada al servicio **QuantumLeap**.
   - La suscripción se crea con un `POST` a `/v2/subscriptions`.
4. **Carga de datos:**
   - Ahora viene la carga de los datos masivos. Utilizaremos el script de Python que creamos en el Módulo 2.
   - El script se modificará para **enviar 400 actualizaciones por atributo** a la API de Orion para cada una de nuestras 3 entidades.
   - Cada vez que el script envíe una actualización, Orion hará dos cosas:
     - Actualizará el documento en **MongoDB** con el nuevo estado.
     - Debido a la suscripción, enviará una copia de los datos a **QuantumLeap**, que a su vez los insertará en **CrateDB** como una nueva entrada de serie de tiempo.

Al final de la práctica, tendrás un sistema funcional donde puedes ver el estado actual de tus entidades en MongoDB y consultar el historial completo en CrateDB.