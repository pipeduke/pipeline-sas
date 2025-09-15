# pipeline-sas
Simulación de pipeline bancario con SAS + Python + AWS.

# Data Pipeline – SAS + Python + AWS (Simulación)

Este proyecto simula un **pipeline bancario de datos** como los que se desarrollan en empresas de consultoría tecnológica para el sector financiero.  
Incluye **ingesta de datos, limpieza, transformación, KPIs y visualización**, usando **SAS, Python y AWS**.

---

## 🎯 Objetivos del proyecto
- Construir un **pipeline de datos de clientes y transacciones** bancarias.  
- Practicar el uso de **SAS Base, Enterprise Guide y PROC SQL**.  
- Integrar Python como orquestador/ETL (simulación de AWS S3).  
- Generar **reportes y dashboards** para stakeholders de negocio.  
- Demostrar **buenas prácticas de versionado y documentación con Git**.  

---

## 📂 Estructura del repositorio

pipeline-sas/
│
├── data/ # Datos de prueba (clientes.csv, transacciones.csv)
├── src/
│ ├── sas/ # Código SAS (ingesta, limpieza, KPIs, reportes)
│ ├── python/ # Scripts Python (ETL, integración S3)
│
├── notebooks/ # Notebooks de exploración (EDA, PySpark demo)
├── reports/ # Salidas: tablas SAS, capturas dashboards
├── tests/ # Validaciones de calidad de datos
│
├── README.md # Documentación principal
├── requirements.txt # Dependencias Python

---

## ⚙️ Tecnologías utilizadas

### Lenguajes
- **SAS Base / PROC SQL / DATA step**
- **Python 3 (pandas, boto3, pytest)**
- **SQL (consultas y joins)**
- **(Opcional) Scala/Java** para batch jobs

### Cloud & Orquestación
- **AWS S3** (simulado/local para ingesta)
- **Airflow** (para DAGs de pipelines)
- **Docker** (para empaquetar procesos)

### Visualización
- **Power BI**
- **Looker Studio**

---

## 🔧 Pipeline de datos – Fases

### 1. Ingesta
- Archivos CSV (`clientes.csv`, `transacciones.csv`).  
- En proyecto real, llegarían desde **SFTP, API REST o bucket S3**.  

Ejemplo SAS:

%LET path = /home/uXXXXX;  /* Macrovariable ruta */

DATA clientes;
    INFILE "&path./clientes.csv" DSD FIRSTOBS=2;
    INPUT id_cliente nombre :$30. ciudad :$20. segmento :$15. fecha_registro :yymmdd10.;
    FORMAT fecha_registro yymmdd10.;
RUN;

Ejemplo python

import boto3
s3 = boto3.client("s3")
s3.download_file("bucket", "transacciones/2025-09-01.csv", "data/transacciones.csv")

2. Limpieza

Validación de montos (monto > 0).

Normalización de texto (tipo, canal).

Filtrado de datos inválidos.

Ejemplo SAS:

DATA transacciones_clean;
    SET transacciones;
    IF monto <= 0 THEN DELETE;
    tipo = PROPCASE(tipo);
    canal = PROPCASE(canal);
RUN;

3. Transformación

KPI por cliente: total transacciones.

KPI por ciudad: sumatoria de montos.

Ejemplo PROC SQL:

PROC SQL;
    CREATE TABLE kpi_ciudad AS
    SELECT ciudad, SUM(monto) AS total_ciudad
    FROM transacciones_clean t
    INNER JOIN clientes c ON t.id_cliente = c.id_cliente
    GROUP BY ciudad;
QUIT;

4. Reportes

PROC REPORT → tablas agregadas.

Exportación a CSV/Excel.

Dashboard en Power BI (simulación).

5. Tests de calidad

Ejemplo en Python con pytest:

import pandas as pd

def test_montos_positivos():
    df = pd.read_csv("data/transacciones.csv")
    assert (df["monto"] > 0).all(), "Existen transacciones con monto no válido"

📊 Resultados esperados

Tablas SAS con KPIs (kpi_cliente, kpi_ciudad).

Dashboard con gráficos de:

Total transacciones por ciudad.

Distribución por canal.

Top clientes por monto.

Validaciones automáticas en tests.

📅 Roadmap

 Generar datasets de prueba (clientes + transacciones).

 Cargar y limpiar en SAS.

 KPIs en PROC SQL.

 Integrar Python con S3 simulado.

 Dashboard en Power BI.

 Pipeline completo en Airflow.

👤 Autor

Felipe Duque
Ingeniero en Sistemas y Telecomunicaciones | Analista & Data Engineer.
