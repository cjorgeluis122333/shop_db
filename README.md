# 💾 Realizar un Backup o Restore de una Base de Datos PostgreSQL (en Docker)
Este documento explica cómo **realizar un backup** o **restaurar** una base de datos PostgreSQL dentro de un contenedor Docker, teniendo en cuenta los distintos formatos posibles (`.backup`, `.sql`, `.csv`).

---

## 🧱 Archivos compatibles con PostgreSQL

| Tipo de archivo | Descripción | Herramienta recomendada |
|-----------------|--------------|--------------------------|
| `.backup` / `.dump` | Backup binario generado con `pg_dump` | `pg_restore` |
| `.sql` | Script SQL exportado | `psql` |
| `.csv` | Datos en formato tabular | `psql` con `\COPY` |

---

## 🚀 Restaurar una base de datos desde un archivo `.backup` o `.sql`

### **Paso 1. Copiar el archivo al contenedor**
Copia el archivo de backup desde tu máquina local al contenedor PostgreSQL.  
Ejemplo:

```bash
docker cp "D:\PROGRAMACION\BBD\PosgredSQL\project\shop_db\pgadmin\storage\jcvidal_google.com\para_dario.sql" microservice_shop_project:/tmp/para_dario.sql
```

### ***Paso 2. Restaurar la base de datos***
`dentro de shop_db: <- restaura el backup que copiamos a temp`
```bash
docker exec microservice_shop_project pg_restore -v --clean --no-owner --no-privileges -U jcvidal -d dario /tmp/para_dario.sql
```

### 💡 Detalles de los parámetros
- **`microservice_shop_project`** → nombre del contenedor que ejecuta tu instancia de **PostgreSQL**.  
- **`jcvidal`** → tu usuario de Postgres (definido en POSTGRES_USER).
- **`solicitud_db`** → la base de datos destino (definida en POSTGRES_DB).
- **`/tmp/solicitud.backup`**  → la ruta dentro del contenedor donde copiaste el archivo con docker cp.


## 🚀 Restaurar una base de datos desde un archivo `.csv`
Para archivos no complatibles con posgre lo primero que se hace es tranformar estos archivos a `.csv` que son legibles para posgres pero hay que realizar unos pasos extras

### Paso 1 Verificar contenido del archivo .csv
Se utilizar el comando: **`type "<AbsolutePath>" | more`** para optenr la informacion de dicho archivo y poder crear la tabla
```bash
type "C:\Users\cjorg\Downloads\4to Año\Sistemas de la Informacion\Tare Extraclase practica\Fuentes de datos\7- Cardiocentro\Municipio.csv" | more
```


### Paso 2: Crear la tabla con los datos del archibo
Es nesestario tomar todos los campos que estan en el archivo y apartir de alli crear la tabla

```bash
docker exec -it microservice_shop_project psql -U jcvidal -d cardiocentro -c "
CREATE TABLE municipio (
code INTEGER PRIMARY KEY,
name TEXT,
provincia TEXT,
region TEXT
);"
```

### Paso 3: Hacer Marge: Copiar los datos (Formato UTF-8)
En caso de que los datos del archivo se encuetren en formato UTF-8 con  este comando podras rellenar la tabla al completo
```bash
docker exec -it microservice_shop_project psql -U jcvidal -d cardiocentro -c "\COPY municipio FROM '/tmp/Municipio.csv' DELIMITER ';' CSV HEADER;"
```

### Paso 3: Copiar los datos (Si el formato no es UTF-8)
Si el formato no es UTF-8 el siguiente comando sera el que debas aplicar
```bash
docker exec -it microservice_shop_project psql -U jcvidal -d cardiocentro -c "\COPY municipio FROM '/tmp/Municipio.csv' DELIMITER ';' CSV HEADER ENCODING 'WIN1252';"
```
