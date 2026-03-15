# Ecosistema Sistema de Big Data

Este proyecto implementa un **ecosistema básico de Big Data orientado al análisis de datos**, utilizando herramientas comunes en arquitecturas modernas de datos.

El objetivo es desplegar un entorno donde los datos se almacenan en un **Data Lake**, se consultan mediante **SQL con Trino** y se visualizan mediante **Apache Superset**.

Todo el sistema se despliega mediante **Docker Compose**, lo que permite levantar el entorno completo de forma sencilla y reproducible.

Este proyecto forma parte de una práctica donde se implementa un pipeline de análisis de datos utilizando tecnologías habituales del ecosistema Big Data.

## Arquitectura del sistema

El ecosistema está compuesto por los siguientes componentes:

| Herramienta     | Función                                               |
| --------------- | ----------------------------------------------------- |
| PostgreSQL      | Base de datos utilizada por Hive Metastore            |
| Hive Metastore  | Catálogo de metadatos de las tablas del Data Lake     |
| Trino           | Motor SQL distribuido para consultas analíticas       |
| Apache Superset | Herramienta de visualización y creación de dashboards |

### Flujo de datos

Data Lake  
↓  
Hive Metastore  
↓  
Trino (Motor SQL)  
↓  
Apache Superset  
↓  
Dashboard

### Funcionamiento del pipeline

1. Los datos se almacenan en el **Data Lake**.
2. **Hive Metastore** registra las tablas y sus metadatos.
3. **Trino** permite realizar consultas SQL sobre los datos almacenados.
4. **Superset** se conecta a Trino para visualizar los datos y generar dashboards.

### Funcionamiento del pipeline

1. Los datos se almacenan en el **Data Lake**.
2. **Hive Metastore** registra las tablas y sus metadatos.
3. **Trino** permite realizar consultas SQL sobre los datos almacenados.
4. **Superset** se conecta a Trino para visualizar los datos y generar dashboards.

## Estructura del repositorio

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

### Descripción de carpetas

**superset/**  
Contiene el Dockerfile personalizado de Superset que permite conectarse con Trino.

**trino/etc/**  
Contiene la configuración del motor SQL Trino.

- `catalog/hive.properties` → configuración del catálogo Hive
- `config.properties` → configuración del coordinador
- `node.properties` → configuración del nodo
- `jvm.config` → parámetros de la JVM

**warehouse/**  
Directorio utilizado como Data Lake donde se almacenan los datos.

## Requisitos previos

Para ejecutar este proyecto es necesario tener instalado:

- Docker
- Docker Compose
- Git
- Ubuntu / Windows con WSL2 / macOS

Para verificar la instalación ejecutar:

```bash
docker --version
docker compose version
git --version
```

# BLOQUE 6 — INSTALACIÓN

## Instalación del proyecto

### 1. Clonar el repositorio

```bash
git clone https://github.com/usuario/ecosistema_sistema_de_big_data.git
cd ecosistema_sistema_de_big_data
```

2. Levantar los servicios

docker compose up -d

3. Verificar contenedores

docker ps

Deberían aparecer los siguientes servicios:

- superset
- trino
- hive-metastore
- hive-metastore-db

---

# BLOQUE 7 — ACCESO A LAS HERRAMIENTAS

## Acceso a las herramientas

| Servicio        | URL                   |
| --------------- | --------------------- |
| Apache Superset | http://localhost:8088 |
| Trino UI        | http://localhost:8080 |

## Uso del sistema

### Acceder al cliente SQL de Trino

```bash
docker exec -it trino trino
```

## Uso del sistema

### Acceder al cliente SQL de Trino

```bash
docker exec -it trino trino
```

### Ver catálogos disponibles

```bash
SHOW CATALOGS;
```

#### Ver schemas del catálogo Hive

```bash
SHOW SCHEMAS FROM hive;
```

---

# BLOQUE 9 — CREAR TABLA DE EJEMPLO

## Crear esquema y tabla de ejemplo

### Crear esquema

```bash
CREATE SCHEMA IF NOT EXISTS hive.demo
WITH (location = 'file:///opt/hive/data/warehouse/demo.db');
```

#### Crear tabla

```bash
CREATE TABLE hive.demo.ventas (
    id INTEGER,
    producto VARCHAR,
    importe DOUBLE,
    fecha DATE
)
WITH (format = 'PARQUET');
```

### Insertar datos

```bash
INSERT INTO hive.demo.ventas VALUES
(1, 'Portatil', 899.99, DATE '2026-03-15'),
(2, 'Raton', 25.50, DATE '2026-03-15'),
(3, 'Teclado', 49.99, DATE '2026-03-16'),
(4, 'Monitor', 199.90, DATE '2026-03-16');
```

Consultar datos

```bash
SELECT * FROM hive.demo.ventas;
```

---

# BLOQUE 10 — CONSULTA ANALÍTICA

## Consulta analítica de ejemplo

```bash
SELECT producto, SUM(importe) AS total
FROM hive.demo.ventas
GROUP BY producto
ORDER BY total DESC;
```

Reconstruir la imagen:

```bash
docker compose build
docker compose up -d
```

---

# BLOQUE 13 — PARAR EL SISTEMA

## Parar el sistema

Para detener todos los contenedores ejecutar:

```bash
docker compose down
```

# BLOQUE 14 — VALIDACIÓN

## Validación del sistema

Para comprobar que el sistema funciona correctamente se pueden realizar las siguientes pruebas.

### Trino ejecuta consultas

```bash
SELECT 1;
```

#### Hive Metastore registra tablas

```bash
SHOW TABLES FROM hive.demo;
```

#### Consulta analítica

```bash
SELECT producto, SUM(importe)
FROM hive.demo.ventas
GROUP BY producto;
```

#### Superset visualiza datos

#### Crear un gráfico en Superset usando la tabla:

```bash
hive.demo.ventas
```

---

# BLOQUE 15 — TECNOLOGÍAS

## Tecnologías utilizadas

- Docker
- Docker Compose
- Apache Trino
- Apache Hive Metastore
- PostgreSQL
- Apache Superset
