Ecosistema Sistema de Big Data

Este proyecto implementa un ecosistema básico de Big Data orientado al análisis de datos, utilizando herramientas comunes en arquitecturas modernas de datos.

El objetivo es desplegar un entorno donde los datos se almacenan en un data lake, se consultan mediante SQL con Trino y se visualizan mediante Apache Superset.

Todo el sistema se despliega mediante Docker Compose, permitiendo reproducir fácilmente el entorno completo.

Arquitectura del sistema

El ecosistema está compuesto por los siguientes componentes:

Herramienta Función
PostgreSQL Base de datos utilizada por Hive Metastore
Hive Metastore Catálogo de metadatos de tablas del data lake
Trino Motor SQL distribuido para consultas analíticas
Apache Superset Visualización de datos y creación de dashboards
Flujo de datos
Datos -> Hive Metastore -> Trino -> Superset -> Dashboard

Los datos se almacenan en el data lake.

Hive Metastore registra las tablas y metadatos.

Trino permite consultar los datos mediante SQL.

Superset se conecta a Trino para crear dashboards.

Estructura del repositorio
ECOSISTEMA_SISTEMA_DE_BIG_DATA

├── docker-compose.yml
├── README.md

├── superset
│ └── Dockerfile

├── trino
│ └── etc
│ ├── catalog
│ │ └── hive.properties
│ ├── config.properties
│ ├── jvm.config
│ └── node.properties

└── warehouse
Descripción de carpetas

superset/
Contiene el Dockerfile personalizado para Superset con soporte para Trino.

trino/etc/
Configuración del motor SQL Trino.

catalog/hive.properties → catálogo Hive

config.properties → configuración del coordinador

node.properties → configuración del nodo

jvm.config → parámetros JVM

warehouse/
Directorio donde se almacenan los datos del data lake.

Requisitos previos

Antes de ejecutar el proyecto es necesario tener instalado:

Docker

Docker Compose

Git

Ubuntu / Windows con WSL2 / macOS

Verificar instalación:

docker --version
docker compose version
git --version
Instalación del proyecto

Clonar el repositorio:

git clone https://github.com/usuario/ecosistema_sistema_de_big_data.git
cd ecosistema_sistema_de_big_data

Levantar los servicios:

docker compose up -d

Verificar que los contenedores están activos:

docker ps

Deberían aparecer los siguientes servicios:

superset
trino
hive-metastore
hive-metastore-db
Acceso a las herramientas
Servicio URL
Superset http://localhost:8088

Trino UI http://localhost:8080
Uso del sistema
Acceder a Trino

Entrar al cliente SQL de Trino:

docker exec -it trino trino

Ver catálogos disponibles:

SHOW CATALOGS;

Ver schemas de Hive:

SHOW SCHEMAS FROM hive;
Crear esquema y tabla de ejemplo

Crear el esquema:

CREATE SCHEMA IF NOT EXISTS hive.demo
WITH (location = 'file:///opt/hive/data/warehouse/demo.db');

Crear tabla de ventas:

CREATE TABLE hive.demo.ventas (
id INTEGER,
producto VARCHAR,
importe DOUBLE,
fecha DATE
)
WITH (format = 'PARQUET');

Insertar datos de ejemplo:

INSERT INTO hive.demo.ventas VALUES
(1, 'Portatil', 899.99, DATE '2026-03-15'),
(2, 'Raton', 25.50, DATE '2026-03-15'),
(3, 'Teclado', 49.99, DATE '2026-03-16'),
(4, 'Monitor', 199.90, DATE '2026-03-16');

Consultar datos:

SELECT \* FROM hive.demo.ventas;
Consulta analítica de ejemplo

Ejemplo de agregación de ventas por producto:

SELECT producto, SUM(importe) AS total
FROM hive.demo.ventas
GROUP BY producto
ORDER BY total DESC;
Visualización en Superset

Acceder a:

http://localhost:8088

Conectar Superset con Trino.

Crear un dataset usando:

hive.demo.ventas

Crear un gráfico:

Tipo de gráfico:

Bar Chart

Configuración:

X-axis → producto
Metric → SUM(importe)

Guardar el gráfico.

Crear un dashboard.

Construcción personalizada de Superset

El contenedor de Superset se construye con un Dockerfile personalizado ubicado en:

superset/Dockerfile

Este Dockerfile añade dependencias necesarias para conectar Superset con Trino.

FROM apache/superset:latest

USER root

RUN /usr/local/bin/pip install --target /app/.venv/lib/python3.10/site-packages "SQLAlchemy==1.4.54" \
 && /usr/local/bin/pip install --target /app/.venv/lib/python3.10/site-packages --no-deps "trino[sqlalchemy]"

USER superset

Reconstruir la imagen si se modifica:

docker compose build
docker compose up -d
Parar el sistema

Detener todos los contenedores:

docker compose down
Validación del sistema

Para comprobar que el sistema funciona correctamente se pueden realizar las siguientes pruebas:

1. Trino ejecuta consultas SQL
   SELECT 1;
2. Hive Metastore registra tablas
   SHOW TABLES FROM hive.demo;
3. Consulta analítica
   SELECT producto, SUM(importe)
   FROM hive.demo.ventas
   GROUP BY producto;
4. Superset visualiza datos

Crear un gráfico en Superset usando la tabla hive.demo.ventas.

Tecnologías utilizadas

Docker

Docker Compose

Apache Trino

Apache Hive Metastore

PostgreSQL

Apache Superset
