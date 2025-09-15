Aquí tienes una versión ampliada y detallada del temario de tu módulo, estructurada como apuntes de clase. El contenido está diseñado para ser directo, práctico y fácil de seguir, enfocándose en la visualización y análisis de datos con **Microsoft Power BI**.

------



### **💡 Módulo 7: Analítica de Negocio y Visualización con Power BI**





### **Introducción: La Última Milla del Dato**



Has trabajado duro. Has recolectado datos, los has almacenado, los has procesado y los has automatizado. Pero, ¿de qué sirve todo ese esfuerzo si no podemos entender lo que los datos nos dicen? Este módulo es la culminación de tu viaje en la ciencia de datos. Aquí, convertirás los datos brutos en **conocimiento accionable** utilizando **Microsoft Power BI**, una de las herramientas de **Inteligencia de Negocio (BI)** más poderosas del mercado.

Aprenderás a conectar Power BI a tus fuentes de datos, a modelar la información de forma eficiente y a diseñar paneles interactivos que ayuden a la toma de decisiones. Todo esto lo aplicaremos directamente en el **Proyecto 2**, para analizar los resultados académicos.

------



### **7.1. Fundamentos de Business Intelligence (BI)**



La Inteligencia de Negocio es el proceso de transformar datos brutos en información significativa y útil para el análisis empresarial. Las herramientas de BI como Power BI, Tableau o Qlik, son la interfaz entre los datos y los usuarios de negocio.



#### **¿Por qué usar una herramienta de BI?**



- **Conexión con múltiples fuentes de datos:** Permiten importar datos de bases de datos, archivos, servicios en la nube, etc.
- **Modelado de datos:** Facilitan la creación de un modelo lógico a partir de diferentes tablas.
- **Visualización:** Ofrecen una amplia gama de gráficos y tablas para representar los datos de manera visual y comprensible.
- **Interactividad:** Los dashboards son dinámicos. Puedes usar filtros y segmentaciones para explorar los datos a diferentes niveles.
- **Colaboración:** Permiten compartir informes y cuadros de mando con otros miembros del equipo.

------

Aquí tienes un apartado detallado sobre **Microsoft Power BI** para tus apuntes, siguiendo la estructura y el tono de los módulos anteriores.

------



### **7.5. Profundizando en Microsoft Power BI** 📊



**Microsoft Power BI** es una suite de herramientas de inteligencia de negocio que transforma datos de diversas fuentes en información coherente, visualmente inmersiva y que se puede compartir. Está diseñado para que cualquier persona, sin ser un experto, pueda crear visualizaciones y dashboards interactivos.



#### **Características Clave de Power BI**



- **Conectividad Abundante:** Power BI puede conectarse a cientos de fuentes de datos, incluyendo archivos (Excel, CSV), bases de datos (SQL Server, PostgreSQL), servicios en la nube (Azure, Salesforce) y APIs.
- **Interfaz Intuitiva:** Su interfaz de arrastrar y soltar facilita la creación de informes y dashboards. La curva de aprendizaje es relativamente baja, especialmente para aquellos familiarizados con Microsoft Excel.
- **Potente Modelado de Datos:** Permite limpiar, transformar y relacionar datos de distintas fuentes para crear un modelo unificado, ideal para un análisis más profundo.
- **Lenguaje DAX:** El lenguaje de fórmulas **DAX** (Data Analysis Expressions) es la columna vertebral de Power BI para el análisis. Permite crear métricas personalizadas, columnas calculadas y filtros complejos.
- **Ecosistema Integrado:** Al ser parte de Microsoft, se integra a la perfección con otras herramientas como Excel, Azure y Microsoft 365, lo que lo hace una opción preferida para muchas empresas.



#### **Componentes Principales**



La suite de Power BI se compone de varias partes que trabajan en conjunto:

- **Power BI Desktop:** La herramienta principal para el desarrollo. Es una aplicación de escritorio gratuita donde te conectas a las fuentes de datos, modelas la información y creas los informes y visualizaciones.
- **Servicio Power BI (Power BI Service):** Es la plataforma basada en la nube donde publicas y compartes tus informes y dashboards. Permite la colaboración en equipo, la actualización automática de los datos y la visualización desde cualquier dispositivo.
- **Power BI Mobile:** Aplicaciones para iOS y Android que te permiten ver y consumir los informes y dashboards publicados desde tu teléfono o tablet.



#### **¿Cómo funciona?**



El proceso de trabajo con Power BI generalmente sigue estos tres pasos:

1. **Obtención y Transformación de Datos (Power Query):** Usas Power BI Desktop para conectarte a tus fuentes de datos. Con el editor de **Power Query**, puedes limpiar y transformar los datos (ej. renombrar columnas, eliminar duplicados, cambiar tipos de datos) para prepararlos para el análisis.
2. **Modelado de Datos (Power Pivot):** En la vista de modelo, creas **relaciones** entre las tablas y usas el lenguaje **DAX** para crear medidas y columnas calculadas. Esto crea un modelo de datos lógico y eficiente, optimizado para la analítica.
3. **Visualización y Publicación:** En la vista de informe, arrastras los campos a un lienzo para crear visualizaciones (gráficos de barras, de líneas, mapas). Una vez finalizado el informe, lo publicas en el **Servicio Power BI** para que otras personas puedan verlo y usarlo.



#### **Ejemplo Sencillo de un Informe en Power BI**



Imagina que quieres visualizar los resultados académicos del Proyecto 2.

1. **Conectas:** Usas Power BI Desktop para conectarte a tu base de datos y traer las tablas de `Calificaciones`, `Cursos` y `Familia`.
2. **Modelas:** Creas relaciones en la vista de modelo: `Calificaciones` se relaciona con `Cursos` y `Familia` a través de sus claves primarias y foráneas. Con DAX, creas una medida para calcular el promedio de calificaciones. `Promedio de Calificaciones = AVERAGEX(Calificaciones, Calificaciones[calificacion_final])`
3. **Visualizas:**
   - Arrastras la medida `Promedio de Calificaciones` a un gráfico de barras.
   - Arrastras el campo `familia_nombre` a un eje para ver el promedio por familia.
   - Añades una tarjeta para mostrar el promedio de calificaciones general.
   - Incluyes un gráfico de líneas para la evolución de las calificaciones por curso.

Este sencillo informe te permite ver de inmediato el rendimiento académico de cada familia y cómo evolucionan los resultados con cada curso, lo que demuestra el poder de Power BI para convertir datos en información útil.

### **7.2. Modelado de Datos para BI con Power BI**



El éxito de un informe de Power BI depende en gran medida de un **modelo de datos** bien estructurado.



#### **Conexión a las Fuentes de Datos**



Power BI puede conectarse a un vasto número de fuentes. En nuestra práctica, nos conectaremos a las tablas que generamos en el Data Warehouse en el Módulo 5. Utilizaremos el conector apropiado para bases de datos (como el conector de SQL Server, PostgreSQL, etc.) para importar los datos de las tablas de hechos y dimensiones.



#### **La Importancia del Esquema en Estrella**



El **esquema en estrella**, que ya hemos discutido en el Módulo 6, es la estructura ideal para el análisis de BI. En Power BI, esto se traduce en:

- Una **tabla de hechos** central que contiene los valores numéricos a analizar (ej. `calificacion_final`).
- Varias **tablas de dimensiones** que contienen los atributos que describen los hechos (ej. `familia`, `curso`, `grado`).
- Las tablas de dimensiones se conectan a la tabla de hechos a través de **relaciones** .



#### **Creación de Relaciones, Jerarquías y Columnas Calculadas**



- **Relaciones:** Power BI detecta automáticamente algunas relaciones, pero es crucial que las revises y las crees manualmente si es necesario. Las relaciones definen cómo una tabla filtra a otra. Por ejemplo, una relación entre la tabla de `Familia` y la de `Calificaciones` permite que un filtro por `Familia` muestre solo las calificaciones de esa familia.
- **Jerarquías:** Permiten a los usuarios profundizar en los datos. Puedes crear una jerarquía de tiempo (`Año` -> `Trimestre` -> `Mes` -> `Día`) o una jerarquía de ubicación.
- **Columnas Calculadas:** Con el lenguaje **DAX**, puedes crear nuevas columnas en tu modelo de datos. Por ejemplo, puedes crear una columna `Resultado` que clasifique a los estudiantes como "Aprobado" o "Suspenso" basándose en una calificación final.



#### **Introducción a DAX (Data Analysis Expressions)**



DAX es el lenguaje de fórmulas de Power BI, similar a las fórmulas de Excel, pero mucho más potente. Se usa para crear:

- **Columnas calculadas:** Como en el ejemplo anterior.
- **Medidas:** Son cálculos dinámicos que se evalúan en el contexto de un informe. Por ejemplo, puedes crear una medida para calcular el promedio de las calificaciones o el total de estudiantes por familia. Las medidas son la base de los **indicadores clave de rendimiento (KPIs)**.

------



### **7.3. Principios de Diseño de Dashboards Efectivos**



Un buen diseño es tan importante como un buen análisis. Un dashboard debe contar una historia de forma clara y concisa.

- **Conoce a tu audiencia:** ¿Qué preguntas intentan responder? ¿Necesitan un alto nivel de detalle o una visión general?
- **Usa un diseño simple:** Evita el exceso de información. Utiliza espacios en blanco y agrupa los elementos lógicamente.
- **Elige las visualizaciones correctas:** Un gráfico de barras es excelente para comparar categorías, mientras que un gráfico de líneas es ideal para mostrar tendencias a lo largo del tiempo.
- **Utiliza colores de forma estratégica:** El color puede guiar la mirada del usuario y resaltar información importante.

------



### **7.4. Práctica: Construcción del Dashboard del Proyecto 2**



Esta práctica es el objetivo final de nuestro proyecto y la demostración de todo lo que has aprendido.

1. **Conexión a la Fuente:** Conecta Power BI a la base de datos que contiene las tablas del Data Warehouse (`Calificaciones`, `Familia`, `Curso`, `Grado`, etc.) que hemos generado con NiFi.
2. **Modelo de Datos:** En la vista de modelo de Power BI, establece las relaciones entre la tabla de hechos (`Calificaciones`) y las tablas de dimensiones (`Familia`, `Curso`, `Grado`).
3. **Visualizaciones y Métricas:**
   - **Filtros:** Agrega segmentaciones (`slicers`) para permitir que los usuarios filtren los datos por `Familia`, `Curso` y `Grado`.
   - **Visualizaciones Interactivas:**
     - Un **gráfico de barras** para comparar la calificación promedio por `Familia` o `Curso`.
     - Un **gráfico de líneas** para mostrar la evolución del promedio de calificaciones a lo largo de los cursos.
   - **Métricas Calculadas con DAX:**
     - Una medida para calcular la **calificación promedio** de todos los estudiantes.
     - Una medida para contar el **número total de estudiantes** en el conjunto de datos.
4. **Diseño del Dashboard:** Organiza las visualizaciones, los filtros y las métricas en una página coherente. Utiliza títulos claros y etiquetas para que el dashboard sea fácil de entender.

Al finalizar, tendrás un dashboard funcional y profesional que proporcionará información valiosa sobre los resultados académicos del proyecto, demostrando tu dominio del proceso completo de ingeniería de datos.