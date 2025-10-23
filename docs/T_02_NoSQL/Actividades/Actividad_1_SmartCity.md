<p style="text-align: center; font-size: 32px;">CIPFP Mislata</p>

<p style="text-align: center; font-size: 18px;">Luis García Bonifaz - l.garciabonifaz@edu.gva.es</p>
<p style="text-align: center; font-size: 24px;">BDA  T02 - Bases de Datos NoSQL</p>

---

<p style="text-align: left; font-size: 24px;">Actividad 1</p>

---

### Smart City - Tarea 2: Construcción infraestructura FIWARE

Vamos a construir una solución completa de gestión de datos de IoT.

1. **Configuración del entorno FIWARE:**
    - Utilizaremos **Docker Compose** para levantar los servicios necesarios:
        - `orion`: El Orion Context Broker.
        - `mongo`: La base de datos MongoDB para el estado actual.
        - `quantumleap`: El componente para el histórico.
        - `cratedb`: La base de datos para el histórico de datos.
    - Asegúrate de que todos los contenedores se comuniquen correctamente a través de la red de Docker.
2. **Creación de las 3 entidades:**
    - Usando la **API REST de Orion Context Broker**, crearemos las 3 entidades que definimos en la *Tarea 1: Diseño del ADN de los Sensores*.
    - La llamada API enviará un `POST` a `/v2/entities` con el JSON de cada entidad.
3. **Creación de una suscripción:**
    - Esta es la parte clave para el histórico. Crearemos una **suscripción** en Orion que "escucha" cualquier cambio en nuestras entidades.
    - Cuando Orion detecte una actualización, enviará automáticamente una copia de la entidad modificada al servicio **QuantumLeap**.
    - La suscripción se crea con un `POST` a `/v2/subscriptions`.
4. **Carga de datos:**
    - Ahora viene la carga de los datos masivos. Utilizaremos un script de Python con la API de Orion.
    - El script deberá **enviar 400 actualizaciones por atributo** a la API de Orion para cada una de nuestras 3 entidades.
    - Cada vez que el script envíe una actualización, Orion hará dos cosas:
        - Actualizará el documento en **MongoDB** con el nuevo estado.
        - Debido a la suscripción, enviará una copia de los datos a **QuantumLeap**, que a su vez los insertará en **CrateDB** como una nueva entrada de serie de tiempo.
5. **Consulta Mongodb**
    - Realiza una captura de pantalla con la consulta Mongodb a la coleccion de entidades en la que se vean todos los atributos de las tres entidades 
6. **Guarda** toda la información en tu Repositorio de GitHub
   
Al final de la práctica, tendrás un sistema funcional donde puedes ver el estado actual de tus entidades en MongoDB y consultar el historial completo en CrateDB.

**Ayudas**

- [Mi primera aplicación con FIWARE](https://fiware-training.readthedocs.io/es-mx/latest/casodeestudio/descripcion/){target:_blank}
- [Orion Context Broker](https://fiware-training.readthedocs.io/es-mx/latest/ecosistemaFIWARE/ocb/){target:_blank}
