# Bitácora de Levantamiento, Puesta en Funcionamiento y Evidencia

**Institución:** Instituto Politécnico Nacional (IPN) — Escuela Superior de Cómputo (ESCOM)  
**Asignatura:** Bases de Datos  
**Grupo:** 3BM2  
**Alumno:** Valencia Villanueva Adrian  
**Proyecto Asignado:** Sistema de Visualización de Datos Sísmicos de México  
**Repositorio Base (Upstream):** [gabrielhuav/Seismic-Data-Visualization-System](https://github.com/gabrielhuav/Seismic-Data-Visualization-System)  
**Repositorio Personal (Fork):** [M4VVii/Seismic-Data-Visualization-System](https://github.com/M4VVii/Seismic-Data-Visualization-System)  
**Entorno de Despliegue:** Linux Mint (x86_64) / Docker Engine v24+ / Docker Compose  

## 1. Requisitos del Sistema y Arquitectura de Contenedores

La aplicación opera bajo una arquitectura multicapa containerizada administrada por Docker Compose:

* **Servidor Web & Aplicación:**
  * **Imagen / Contenedor:** `seismic-data-visualization-system-web-1` (PHP 8.2 + Apache 2.4)
  * **Puerto Mapeado:** `0.0.0.0:80 -> 80/tcp`
  * **Directorio Raíz en Contenedor:** `/var/www/html`
* **Gestor de Base de Datos:**
  * **Imagen / Contenedor:** `seismic-data-visualization-system-db-1` (PostgreSQL 17)
  * **Puerto Mapeado:** `0.0.0.0:5433 -> 5432/tcp`
  * **Base de Datos Activa:** `datawarehouse`
  * **Usuario:** `postgres`
* **Red Interna:** `seismic-data-visualization-system_app-network`
* **Volumen Persistente:** `seismic-data-visualization-system_postgres_data`

## 2. Despliegue Paso a Paso

### Paso 1: Clonación del repositorio Fork y navegación
```bash
git clone https://github.com/M4VVii/Seismic-Data-Visualization-System.git
cd Seismic-Data-Visualization-System
```

### Paso 2: Asignación de permisos al volumen local
Para asegurar que el usuario Apache (`www-data`) tenga acceso de lectura a los scripts PHP y escritura para logs:
```bash
chmod -R 777 .
```

### Paso 3: Construcción y levantamiento de servicios con Docker Compose
```bash
docker compose up -d
```

### Paso 4: Verificación de contenedores activos
```bash
docker ps
```

## 3. Registro de Incidencias Técnicas y Soluciones

Durante el proceso de levantamiento local se diagnosticaron y resolvieron cuatro bloqueos técnicos principales:

### Incidencia 1: Error de puerto ocupado (`Bind for 0.0.0.0:5433 failed`)
* **Síntoma:** Al ejecutar `docker compose up -d`, la terminal arrojó `Error response from daemon: driver failed programming external connectivity on endpoint ... Bind for 0.0.0.0:5433 failed: port is already allocated`.
* **Causa Raíz:** Instancias previas de contenedores o servicios locales mantenían ocupado el puerto host 5433.
* **Solución Aplicada:** Detención de todos los contenedores activos antes de reestructurar la red:
  ```bash
  docker stop $(docker ps -q)
  docker compose up -d
  ```

### Incidencia 2: Respuesta HTTP `403 Forbidden` al acceder a `http://localhost`
* **Síntoma:** El navegador mostraba `Forbidden: You don't have permission to access this resource. Apache/2.4.69 (Debian) Server at localhost Port 80`.
* **Causa Raíz:** El `Dockerfile` copió los scripts del proyecto desde la carpeta `src/` directamente a la raíz `/var/www/html`. Al no existir un archivo llamado `index.php` o `index.html` en la raíz, Apache bloqueó el listado automático del directorio.
* **Solución Aplicada:** Verificación del contenido de Apache con `docker exec -it seismic-data-visualization-system-web-1 ls -la /var/www/html` e ingreso directo a las vistas del sistema:
  * `http://localhost/vista.html`
  * `http://localhost/sismos.php`

### Incidencia 3: Error de conexión a la Base de Datos (`database "dwsismos" does not exist`)
* **Síntoma:** La aplicación PHP arrojaba el mensaje `Warning: pg_connect(): Unable to connect to PostgreSQL server` y los comandos iniciales en consola devolvían `FATAL: database "dwsismos" does not exist`.
* **Causa Raíz:** Se comprobó mediante `psql -c "\l"` que el nombre real con el que se inicializó la base de datos en la imagen Docker era **`datawarehouse`**.
* **Solución Aplicada:** Redirección de todas las cadenas de conexión y comandos `psql` hacia la base de datos `datawarehouse`.

### Incidencia 4: Tablas vacías (`0 rows`) y caída del contenedor por duplicidad de claves
* **Síntoma:** Consultas a `dim_sismos` devolvían `(0 rows)`. Al intentar inyectar datos manualmente sobre un contenedor existente, las claves primarias duplicadas provocaron fallos (`duplicate key value violates unique constraint "dim_tiempo_pkey"`) y la detención del contenedor (`container is not running`).
* **Solución Aplicada:** Reinicio con volumen limpio y recarga estructurada de archivos SQL:
  ```bash
  # 1. Limpieza de volúmenes corruptos
  docker compose down -v
  docker compose up -d

  # 2. Creación e inyección ordenada de scripts SQL
  docker exec -i seismic-data-visualization-system-db-1 psql -U postgres -c "DROP DATABASE IF EXISTS datawarehouse;"
  docker exec -i seismic-data-visualization-system-db-1 psql -U postgres -c "CREATE DATABASE datawarehouse;"

  for f in sql/*.sql; do
    echo "Cargando $f..."
    docker exec -i seismic-data-visualization-system-db-1 psql -U postgres -d datawarehouse < "$f"
  done
  ```

## 4. Validación de la Base de Datos y Consultas SQL

Tras completar la ingesta de los archivos SQL (`01-schema.sql` a `05-fact_impacto_sismos_imputed.sql`), se ejecutó la consulta oficial para extraer los 5 sismos de mayor magnitud registrados en PostgreSQL:

```sql
SELECT s.id_sismo, s.magnitud, s.referencia_de_localizacion 
FROM dim_sismos s 
ORDER BY s.magnitud DESC 
LIMIT 5;
```

### Resultado Ejecutado en PostgreSQL (`datawarehouse`):
```text
 id_sismo | magnitud |          referencia_de_localizacion          
----------+----------+----------------------------------------------
       55 |      8.2 | 4 km al SURESTE de CASIMIRO CASTILLO, JAL
     2461 |      8.1 | 45 km al NOROESTE de LA MIRA, MICH
     9019 |        8 | 10 km al SURESTE de MANZANILLO, COL
       79 |      7.9 | 93 km al ESTE de CD HIDALGO, CHIS
       56 |      7.8 | 14 km al SUROESTE de TUXPAN, JAL
(5 rows)
```

## 5. Catálogo de Evidencias Fotográficas

Las tres evidencias oficiales requeridas por la guía de la Práctica 2 fueron almacenadas en el directorio `P2/proyecto-asignado/evidencias/`:

* **`01-terminal-docker.webp`**: Captura de terminal que valida el arranque correcto de los dos contenedores mediante `docker ps`, confirmando los mapeos en puertos `80` (web) y `5433` (PostgreSQL).
* **`02-aplicacion-local.webp`**: Captura de pantalla del navegador Brave cargando `http://localhost/vista.html`, mostrando el mapa dinámico con áreas de afectación en Guerrero y Oaxaca, contabilizando **3 sismos**, **11,804,981 personas potencialmente afectadas** y un sismo con impacto máximo de **magnitud 6.7**.
* **`03-consulta-sql.webp`**: Captura de terminal ejecutando el comando `docker exec -it ... psql` mostrando la tabla formal de los 5 sismos más fuertes de la base de datos `datawarehouse`.