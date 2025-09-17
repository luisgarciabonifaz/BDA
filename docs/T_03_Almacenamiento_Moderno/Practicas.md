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