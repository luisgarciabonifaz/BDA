¡Excelente! Continuamos con el desarrollo del curso. Aquí tienes la propuesta de apuntes para el **Módulo 2**, siguiendo el mismo formato didáctico y práctico.

------



## **Apuntes de Clase: Módulo 2 - Python para Data Engineering**





### **Introducción al Módulo**



Si el Big Data fuera una gran obra de construcción, Python sería la navaja suiza que todo ingeniero lleva en su cinturón. Es el lenguaje que nos permite automatizar tareas, conectar sistemas que no hablan entre sí, y lo más importante: **manipular y limpiar los datos**. Un ingeniero de datos pasa entre el 60% y el 80% de su tiempo en tareas de limpieza y preparación de datos. Dominar las herramientas para hacerlo eficientemente es, por tanto, una habilidad crucial.

En este módulo, nos sumergiremos en **Pandas**, la librería que ha convertido a Python en el rey del análisis de datos. Aprenderemos a cargar, inspeccionar, limpiar y transformar datos tabulares. Al final, aplicaremos estos conocimientos para dar los primeros pasos prácticos en nuestros dos proyectos: pondremos orden en los datos académicos y construiremos un simulador para los sensores de nuestra Smart City. ¡Vamos a ensuciarnos las manos con código!

------



### **2.1. Python como Lenguaje Vehicular: ¿Por qué Python?**



Python no es el único lenguaje de programación, pero es el dominante en el mundo de los datos por varias razones clave:

- **Sintaxis Sencilla y Legible:** Su código se parece mucho al inglés, lo que facilita su aprendizaje y permite escribir programas de forma rápida y clara.
- **Ecosistema de Librerías Gigante:** La verdadera magia de Python reside en sus librerías especializadas. Para datos, tenemos:
  - **Pandas:** Para manipulación de datos tabulares (nuestro foco principal).
  - **NumPy:** Para cálculo numérico y operaciones matemáticas eficientes.
  - **Requests:** Para comunicarnos con APIs y obtener datos de la web.
  - Y muchas más (Scikit-learn para Machine Learning, Matplotlib para visualización, etc.).
- **Gran Comunidad y Soporte:** Si tienes un problema, es casi seguro que alguien ya lo ha tenido y hay una solución en internet (en foros como Stack Overflow, por ejemplo).
- **Integración ("Glue Language"):** Python es conocido como un "lenguaje pegamento" porque puede conectar fácilmente diferentes tecnologías: bases de datos, servicios en la nube, APIs, etc.

Nuestro Entorno de Trabajo: Jupyter Notebooks

Para este módulo, usaremos Jupyter Notebooks (o un entorno similar como VS Code con la extensión de Python). Son herramientas interactivas que nos permiten ejecutar código en celdas individuales y ver el resultado inmediatamente, mezclando código, texto y gráficos. Son el estándar de facto para la exploración y el análisis de datos.

------



### **2.2. Librería Pandas: El Corazón del Análisis de Datos 🐼**



Pandas es una librería que nos proporciona estructuras de datos y herramientas para trabajar con datos tabulares (como los de un fichero CSV o una tabla de SQL) de una forma muy intuitiva y potente. ¡Es como tener superpoderes en Excel!



#### **Estructuras de Datos Fundamentales**



- **`Series`**: Es un array de una dimensión, similar a una única columna de una tabla. Cada elemento tiene un índice asociado.

  Python

  ```
  import pandas as pd
  
  # Una Serie de notas
  notas = pd.Series([7, 5, 9, 10], index=['Mates', 'Lengua', 'Inglés', 'Historia'])
  print(notas)
  ```

- **`DataFrame`**: Es una tabla de dos dimensiones (filas y columnas). Es la estructura de datos más importante y la que usaremos constantemente. Podemos pensar en ella como una colección de `Series`.

  Python

  ```
  # Un DataFrame de alumnos
  datos_alumnos = {
      'nombre': ['Ana', 'Juan', 'Eva'],
      'edad': [22, 25, 23],
      'ciudad': ['Valencia', 'Madrid', 'Valencia']
  }
  df_alumnos = pd.DataFrame(datos_alumnos)
  print(df_alumnos)
  ```



#### **El Índice: La Columna Vertebral de Pandas**



El índice es uno de los conceptos más potentes de Pandas. No es solo un número de fila; es una etiqueta que permite una alineación de datos increíblemente rápida y un acceso eficiente. Aunque no lo modifiquemos, siempre está ahí. Más adelante, aprenderemos a establecer nuestras propias columnas como índice (por ejemplo, una columna de fecha) para realizar análisis de series temporales de forma muy eficiente.

#### **Operaciones Fundamentales: El Flujo de Trabajo del Analista**



1. Carga de Datos

   El primer paso es siempre leer los datos desde una fuente. La función más común es pd.read_csv().

   Python

   ```
   # Ejemplo de carga de datos más robusta
   df = pd.read_csv(
       'datos/fichero_complejo.csv',
       sep=';',                  # Separador de columnas
       decimal=',',              # Caracter usado para los decimales
       encoding='utf-8',         # Codificación del fichero para evitar errores con tildes o caracteres especiales
       dtype={'id_cliente': str} # Forzar que una columna se lea como texto (muy útil para IDs que parecen números pero no lo son)
   )
   ```

2. Diagnóstico y Exploración Profunda

   Una vez cargados los datos, nunca asumimos que están bien. Hacemos una revisión rápida:

   Python

   ```
   # Ver las 5 primeras filas
   print(df_notas.head())
   
   # Ver un resumen técnico: columnas, tipos de datos, valores no nulos
   print(df_notas.info())
   
   # Ver un resumen estadístico de las columnas numéricas
   print(df_notas.describe())
   
   # Ver el número de filas y columnas (forma del DataFrame)
   print(df_notas.shape)
   ```

   Antes de cambiar nada, hay que ser un buen detective.

   - `df.info(verbose=True)`: Nos da un resumen completo. Debemos fijarnos en:
     - **Dtype (Tipo de dato):** ¿Una columna que debería ser numérica aparece como `object`? Esto indica que hay valores de texto mezclados que debemos limpiar.
     - **Non-Null Count (Conteo de no nulos):** ¿Coincide con el número total de filas? Si no, ya sabemos que tenemos valores nulos que tratar.
   - `df.value_counts('nombre_columna')`: Es una de las herramientas de diagnóstico más útiles. Nos muestra la frecuencia de cada valor en una columna. Es perfecta para detectar:
     - Errores tipográficos (ej. "Valencia", "valencia", "Valensia").
     - Valores inesperados en columnas categóricas.

   Python

   ```
   # Ejemplo de uso de value_counts para la columna 'Turno' del Proyecto 2
   print(df_alumnos['Turno'].value_counts())
   # Output esperado:
   # Diurno           500
   # Semipresencial   150
   # Name: Turno, dtype: int64
   
   # Si viéramos "diurno" en minúsculas, ya habríamos detectado un problema de consistencia.
   ```

   

3. **Selección y Filtrado (Acceder a lo que nos interesa)**

   - **Seleccionar Columnas:**

     Python

     ```
     # Seleccionar una columna (devuelve una Serie)
     nombres = df_alumnos['nombre']
     
     # Seleccionar varias columnas (devuelve un DataFrame)
     nombre_y_edad = df_alumnos[['nombre', 'edad']]
     ```

   - **Filtrar Filas por Condición:**

     Python

     ```
     # Filtrar alumnos de Valencia
     alumnos_valencia = df_alumnos[df_alumnos['ciudad'] == 'Valencia']
     
     # Filtrar alumnos mayores de 22 años
     mayores_de_22 = df_alumnos[df_alumnos['edad'] > 22]
     
     # Combinar condiciones (& para 'Y', | para 'O')
     ana_o_eva = df_alumnos[(df_alumnos['nombre'] == 'Ana') | (df_alumnos['nombre'] == 'Eva')]
     ```

4. **Limpieza de Datos (El pan de cada día)**

   - **Manejo de Valores Nulos (`NaN`):**

     Python

     ```
     # Contar cuántos valores nulos hay en cada columna
     print(df_notas.isnull().sum())
     
     # Opción 1: Eliminar las filas que tengan algún valor nulo
     df_sin_nulos = df_notas.dropna()
     
     # Opción 2: Rellenar los nulos con un valor (ej. 0 para las notas)
     df_notas_relleno = df_notas.fillna(value={'nota': 0})
     ```

   - Corrección de Tipos de Datos:

     A veces, un número se lee como texto. Con astype() lo corregimos.

     Python

     ```
     # Imagina que la columna 'id_alumno' se lee como texto
     df_notas['id_alumno'] = df_notas['id_alumno'].astype(int)
     ```

   - **Crear y Modificar Columnas:**

     Python

     ```
     # Crear una nueva columna
     df_alumnos['año_nacimiento'] = 2025 - df_alumnos['edad']
     ```

   - Guardar el resultado:

     Tras la limpieza, guardamos el resultado en un nuevo fichero.

     Python

     ```
     # Guardamos el DataFrame limpio. index=False evita que se guarde el índice de Pandas como una columna.
     df_notas_relleno.to_csv('datos/calificaciones_limpio.csv', index=False)
     ```

------

- **Eliminar Duplicados:**

  Python

  ```
  # Muestra el número de filas antes de eliminar duplicados
  print(f"Filas antes: {len(df)}")
  df_sin_duplicados = df.drop_duplicates()
  print(f"Filas después: {len(df_sin_duplicados)}")
  ```

- **Estrategias de `fillna`:**

  Python

  ```
  # Rellenar con un valor estático
  df['columna'].fillna(0)
  
  # Rellenar con el valor anterior o posterior (útil en series de tiempo)
  df['columna'].fillna(method='ffill') # Forward fill
  
  # Rellenar con la media de la columna (muy común)
  media = df['columna_numerica'].mean()
  df['columna_numerica'].fillna(media)
  ```

- **Manipulación de Texto (`.str`):** El accesor `.str` nos da acceso a decenas de funciones para trabajar con texto.

  Python

  ```
  # Poner en minúsculas
  df['nombre'] = df['nombre'].str.lower()
  
  # Eliminar espacios en blanco al principio y al final
  df['nombre'] = df['nombre'].str.strip()
  
  # Reemplazar texto
  df['curso'] = df['curso'].str.replace('1º', 'Primero')
  
  # Extraer patrones con expresiones regulares (regex)
  # Imagina una columna 'codigo' con formato 'LETTERS-NUMBERS' (ej. 'DAM-101')
  df['numero_codigo'] = df['codigo'].str.extract(r'-(\d+)')
  ```

------



### 

### **2.3. Conexión a Fuentes de Datos (Más allá del CSV)**



- **JSON (JavaScript Object Notation):** Es un formato de texto ligero para el intercambio de datos, muy usado en APIs web. Es como un diccionario de Python.

  Python

  ```
  import json
  
  # Guardar un diccionario a un fichero JSON
  mi_diccionario = {'nombre': 'sensor01', 'valor': 25}
  with open('sensor.json', 'w') as f:
      json.dump(mi_diccionario, f)
  ```

- #### **Manejando la Complejidad de JSON con `json_normalize`**

  Los datos de APIs a menudo vienen en formato JSON anidado (diccionarios dentro de diccionarios). Pandas tiene una función mágica para "aplanar" estos datos y convertirlos en una tabla.

  Python

  ```
  # Ejemplo de JSON anidado
  data = [{'id': 1, 'nombre': 'Ana', 'direccion': {'calle': 'Mayor', 'numero': 10}},
          {'id': 2, 'nombre': 'Juan', 'direccion': {'calle': 'Sol', 'numero': 5}}]
  
  # Sin normalizar, la columna 'direccion' sería un diccionario, lo cual es difícil de usar.
  # df = pd.DataFrame(data)
  
  # Usando json_normalize
  df_normalizado = pd.json_normalize(data)
  print(df_normalizado)
  # Output:
  #    id nombre direccion.calle  direccion.numero
  # 0   1    Ana           Mayor                10
  # 1   2   Juan             Sol                 5
  ```

  

  #### 

- **APIs (Application Programming Interfaces):** Son "enchufes" que exponen los sistemas para que otros programas puedan conectarse y obtener datos de forma controlada. Usaremos la librería `requests` para "llamar" a estas APIs.

  Python

  ```
  import requests
  
  # Ejemplo: Llamar a una API que nos da datos del tiempo en Valencia
  # (Esto es un ejemplo, la URL puede no funcionar siempre)
  response = requests.get('https://goweather.herokuapp.com/weather/Valencia')
  
  # Convertir la respuesta JSON a un diccionario de Python
  datos_tiempo = response.json()
  print(datos_tiempo)
  ```

------

#### **Interactuando con APIs de Forma Segura**



Cuando llamamos a una API con `requests.get()`, la respuesta contiene un **código de estado** que nos dice si todo fue bien.

- `200 OK`: ¡Éxito! La petición se completó correctamente.
- `404 Not Found`: El recurso que pediste no existe.
- `403 Forbidden` / `401 Unauthorized`: No tienes permisos para acceder.
- `500 Internal Server Error`: Hubo un problema en el servidor de la API.

Es una buena práctica comprobar siempre el código de estado antes de intentar usar los datos.

Python

```
response = requests.get('https://api.example.com/data')

if response.status_code == 200:
    data = response.json()
    print("Datos obtenidos con éxito.")
else:
    print(f"Error al llamar a la API: Código {response.status_code}")
```



### **2.4. Práctica: Construyendo Nuestros Scripts 💻**



#### **Práctica 1 (Proyecto 2): Limpieza Inicial de Datos Académicos**



**Objetivo:** Aplicar el flujo de trabajo de Pandas para hacer una primera limpieza de uno de los ficheros CSV del proyecto académico.

Python

```
# Fichero: limpiar_calificaciones.py

import pandas as pd

# 1. CARGA DE DATOS
print("Cargando datos de calificaciones...")
# Asumimos que el separador es ';' y que algunos números pueden tener ',' como decimal
df = pd.read_csv('datos_proyecto/Calificaciones.csv', sep=';', decimal=',')

# 2. INSPECCIÓN INICIAL
print("--- Información Inicial del DataFrame ---")
df.info()
print("\n--- Valores Nulos por Columna ---")
print(df.isnull().sum())

# 3. LIMPIEZA
print("\nIniciando limpieza...")
# Rellenamos las calificaciones nulas con 0, asumiendo que un nulo es un no presentado
df['Calificacion'] = df['Calificacion'].fillna(0)

# Convertimos la columna de calificación a tipo numérico (float)
df['Calificacion'] = df['Calificacion'].astype(float)

# Convertimos la columna de fecha a tipo datetime
df['Fecha'] = pd.to_datetime(df['Fecha'], dayfirst=True)

# 4. VERIFICACIÓN FINAL
print("\n--- Información Final del DataFrame ---")
df.info()
print("\n--- Valores Nulos Restantes ---")
print(df.isnull().sum())
print("\nLimpieza completada.")

# 5. GUARDADO
output_path = 'datos_procesados/Calificaciones_limpio.csv'
print(f"Guardando fichero limpio en: {output_path}")
df.to_csv(output_path, index=False, sep=';')
```

#### **Limpieza Detallada de Datos Académicos**



Python

```
# Fichero: limpiar_calificaciones_detallado.py
import pandas as pd

# --- PASO 1: CARGA DE DATOS ---
# Se utiliza un bloque try-except para manejar el error si el fichero no se encuentra.
try:
    df = pd.read_csv('datos_proyecto/Calificaciones.csv', sep=';', decimal=',')
except FileNotFoundError:
    print("Error: El fichero 'Calificaciones.csv' no se encontró. Verifica la ruta.")
    exit()

# --- PASO 2: DIAGNÓSTICO ---
# Una buena práctica es imprimir la forma del DataFrame para saber con cuántos datos empezamos.
print(f"El fichero tiene {df.shape[0]} filas y {df.shape[1]} columnas.")
# .info() nos da el mapa de nuestros datos: tipos y nulos.
df.info()

# --- PASO 3: LIMPIEZA Y TRANSFORMACIÓN ---
# Columna 'Calificacion': Los nulos pueden significar "No Presentado". Llenarlos con 0 es una decisión de negocio.
df['Calificacion'] = df['Calificacion'].fillna(0)
# Aseguramos que la columna es de tipo numérico para poder hacer cálculos con ella.
df['Calificacion'] = df['Calificacion'].astype(float)

# Columna 'Fecha': Convertir texto a un objeto de fecha real nos permite hacer análisis temporales.
# 'dayfirst=True' le dice a Pandas que el formato es DD/MM/AAAA.
df['Fecha'] = pd.to_datetime(df['Fecha'], dayfirst=True, errors='coerce')
# 'errors='coerce'' convertirá cualquier fecha con formato incorrecto en 'NaT' (Not a Time), que podremos detectar.

# --- PASO 4: CREACIÓN DE NUEVAS COLUMNAS (Enriquecimiento) ---
# Vamos a crear una columna categórica 'Estado' para simplificar el análisis futuro.
# Aquí usamos una función lambda, una forma compacta de definir una función.
df['Estado'] = df['Calificacion'].apply(lambda nota: 'Aprobado' if nota >= 5 else 'Suspenso')

# --- PASO 5: VERIFICACIÓN FINAL ---
print("\n--- DataFrame después de la limpieza y enriquecimiento ---")
print(df.head())
print("\nTipos de datos finales:")
df.info()

# --- PASO 6: GUARDADO ---
# Guardar el resultado en un nuevo fichero es crucial para no sobrescribir los datos originales.
output_path = 'datos_procesados/Calificaciones_limpio.csv'
df.to_csv(output_path, index=False, sep=';', decimal=',')
print(f"\nProceso completado. Fichero limpio guardado en: {output_path}")
```

#### **Práctica 2 (Proyecto 1): Simulador de Sensores de la Smart City**



**Objetivo:** Crear un script de Python que genere datos de sensores simulados y los guarde en un fichero JSON, listos para ser ingeridos en el siguiente módulo.

Python

```
# Fichero: generar_datos_sensores.py

import json
import random
from datetime import datetime, timedelta

# 1. CONFIGURACIÓN
SENSOR_IDS = {
    "temp_hum": "urn:ngsi-ld:TemperatureSensor:001",
    "co2": "urn:ngsi-ld:CO2Sensor:001",
    "agua": "urn:ngsi-ld:WaterQualitySensor:001"
}
FECHA_INICIO = datetime(2025, 3, 1)
NUM_DIAS = 61 # Marzo y Abril
LECTURAS_POR_DIA = round(400 / NUM_DIAS)

# 2. FUNCIÓN PARA GENERAR UNA LECTURA
def crear_lectura_sensor(sensor_id, timestamp):
    lectura = {
        "id": sensor_id,
        "timestamp": timestamp.isoformat() # Formato estándar ISO 8601
    }
    if sensor_id == SENSOR_IDS["temp_hum"]:
        lectura["type"] = "TemperatureSensor"
        lectura["temperature"] = round(random.uniform(15.0, 30.0), 2)
        lectura["humidity"] = round(random.uniform(40.0, 75.0), 2)
    elif sensor_id == SENSOR_IDS["co2"]:
        lectura["type"] = "CO2Sensor"
        lectura["co2"] = random.randint(400, 1200) # en ppm
    elif sensor_id == SENSOR_IDS["agua"]:
        lectura["type"] = "WaterQualitySensor"
        lectura["ph"] = round(random.uniform(6.5, 8.5), 2)
        lectura["temperature"] = round(random.uniform(12.0, 22.0), 2)
        lectura["chlorine"] = round(random.uniform(0.5, 1.5), 3)
    return lectura

# 3. BUCLE DE GENERACIÓN
print("Generando datos simulados para los sensores...")
datos_generados = []
current_date = FECHA_INICIO

for _ in range(NUM_DIAS * LECTURAS_POR_DIA):
    # Generamos una lectura para cada tipo de sensor en un momento aleatorio del día
    for sensor_key, sensor_id in SENSOR_IDS.items():
        # Añadimos una variación aleatoria de tiempo para que no sean todas a la vez
        random_seconds = random.randint(0, 86400) # Segundos en un día
        timestamp = FECHA_INICIO + timedelta(seconds=random_seconds + (_ * 86400 / LECTURAS_POR_DIA))
        if timestamp < FECHA_INICIO + timedelta(days=NUM_DIAS):
            datos_generados.append(crear_lectura_sensor(sensor_id, timestamp))

# 4. GUARDADO EN FICHERO JSON
output_path = 'datos_generados/sensor_data.json'
print(f"Generadas {len(datos_generados)} lecturas.")
print(f"Guardando datos en: {output_path}")

with open(output_path, 'w') as f:
    json.dump(datos_generados, f, indent=4)

print("¡Proceso completado!")
```