<div class="titulo">
   Visualización en Tiempo Real
</div>
#  

A lo largo del curso, hemos construido pipelines de datos para su posterior análisis. Sin embargo, para datos que cambian constantemente, como los de nuestro proyecto de **Smart City**, necesitamos herramientas que puedan mostrar información en **tiempo real** o casi real. Este módulo final se centra en la monitorización y visualización dinámica con **Grafana**, la plataforma líder para este tipo de tareas.

Cerraremos el curso no solo construyendo un panel de control interactivo para nuestro proyecto, sino también consolidando todo lo aprendido a través de la documentación y presentación de ambos proyectos. Esto demostrará tu dominio de todo el ciclo de vida del dato.

## 1. Herramientas para Dashboards en Tiempo Real

Las herramientas de BI tradicionales como Power BI o Tableau son excelentes para la analítica histórica y el análisis de negocio. Sin embargo, cuando se trata de datos de **series temporales** que llegan a alta velocidad y necesitas ver su evolución en un instante, las herramientas de monitorización son la elección correcta.

### 1.1. Grafana: El Estándar para Métricas y Series Temporales

**Grafana** es una plataforma de código abierto de analítica y monitorización. Su principal fortaleza es su capacidad para crear **dashboards dinámicos** que se actualizan automáticamente. Es compatible con una gran cantidad de fuentes de datos (Data Sources), lo que la convierte en una herramienta versátil para visualizar datos de sensores, logs de servidores y cualquier otro dato de series temporales.

<figure markdown="span">
   ![Imagen de Grafana dashboard](https://encrypted-tbn2.gstatic.com/licensed-image?q=tbn:ANd9GcSDcSWmJbE67kLuqBIMx8Ho0bpAaMnSh-hdYY-bmZnVxC3ntwgM8fnPyVCXt6O_jDuU9Q75LqBTLyyVmEzBAfoHRRn5vNCo3i60TUV2m21A8CnULCI){: style="width:70%;margin:auto;display:block"}
  <figcaption>Con licencia de Google</figcaption>
</figure>


A diferencia de las herramientas de BI que se enfocan en responder preguntas de negocio, Grafana se enfoca en **mostrar el estado actual y la evolución de un sistema**, lo que es perfecto para nuestro proyecto de IoT.


## 2. Conceptos Clave en Grafana

Para construir dashboards en Grafana, es fundamental entender sus componentes básicos:

- **Data Source (Fuente de Datos):** Es la conexión a la base de datos de donde Grafana obtendrá los datos. Puede ser una base de datos de series temporales (como Prometheus o InfluxDB), una base de datos relacional (PostgreSQL), o, en nuestro caso, la base de datos histórica de FIWARE (`CrateDB` o `TimescaleDB`).
- **Queries (Consultas):** Las consultas son la forma en que Grafana le pide datos a la fuente de datos. Las consultas se escriben en el lenguaje de la base de datos (por ejemplo, SQL). Grafana ejecuta estas consultas de forma periódica para actualizar los paneles.
- **Panels (Paneles):** Los paneles son los bloques de construcción de un dashboard. Cada panel es una visualización individual (un gráfico de líneas, un medidor, una tabla, etc.) que muestra los datos de una consulta específica.
- **Variables:** Las variables son una de las características más poderosas de Grafana. Te permiten crear un **dashboard dinámico** que se adapta a las selecciones del usuario. Por ejemplo, puedes crear una variable de tipo "Sensor" que muestre un menú desplegable con todos los IDs de sensores, y al seleccionarlo, el dashboard completo se actualice para mostrar solo los datos de ese sensor.
- **Alertas:** Las alertas te permiten definir reglas sobre tus datos. Si un valor supera un umbral, Grafana puede enviar una notificación a un canal de tu elección (ej. email, Slack, etc.). Esto es crucial para la monitorización proactiva de sistemas.


## 3. Práctica: Construcción del Dashboard de Smart City

En esta práctica, construiremos un dashboard de monitorización para los datos de los sensores de nuestro proyecto de Smart City, demostrando la utilidad de los datos en tiempo real.

1. **Conexión a la Fuente de Datos:** En Grafana, añadiremos una nueva fuente de datos. Seleccionaremos el conector adecuado y configuraremos los detalles de conexión a la base de datos donde FIWARE almacena el historial de datos de los sensores (`CrateDB` o `TimescaleDB`).
2. **Creación del Panel de Control:** Construiremos un dashboard que incluya al menos 4 visualizaciones clave:
    - Un **gráfico de líneas** que muestre la evolución de la temperatura a lo largo del tiempo.
    - Un **gráfico de barras** para comparar el nivel de humedad entre los diferentes sensores.
    - Un **medidor** (gauge) que muestre el valor actual de CO2 con un umbral de colores (ej. verde para valores normales, rojo para valores altos).
    - Un **gráfico de series de tiempo** que muestre múltiples métricas de diferentes sensores en un mismo panel.
3. **Implementación de Variables:** Crearemos una variable de tipo **Query** para que el usuario pueda seleccionar el sensor a visualizar de una lista desplegable. Esto hará que el dashboard sea interactivo y útil para el análisis individual de cada sensor.
4. **Configuración de una Alerta:** Configurarás una alerta en el panel de temperatura para que se dispare si el valor supera los 28°C. Definiremos una condición y un canal de notificación (por ejemplo, a través de email).


## 4. Documentación y Presentación de Proyectos

Esta es la etapa final, donde consolidarás y demostrarás todo lo que has aprendido.

1. **Elaboración del Documento Final:** Redactarás un informe detallado para ambos proyectos. Este documento debe incluir:
   - La **arquitectura completa** de cada proyecto, con diagramas que muestren cómo fluyen los datos (desde los sensores o los archivos hasta las herramientas de visualización).
   - Una descripción de cada módulo y las tecnologías utilizadas.
   - Las decisiones de diseño clave (ej. por qué elegiste MongoDB en lugar de una base de datos relacional para el estado actual de FIWARE).
   - Los resultados finales y los dashboards creados.
2. **Presentación Final de Resultados:** Expondrás tus proyectos, guiando a la audiencia a través del ciclo de vida de los datos, desde la ingesta hasta la visualización. Usarás los dashboards que construiste para mostrar el valor del análisis y la monitorización de datos.

Este módulo te proporciona las herramientas para cerrar el ciclo del dato, demostrando tu capacidad para no solo procesar datos, sino también para presentarlos de manera efectiva y con propósito.