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
docker cp "C:\Users\cjorg\Downloads\BaseCardiocentro.backup" microservice_shop_project:/tmp/BaseCardiocentro.backup
```


### ***Paso 2. Restaurar la base de datos***
   
`dentro de shop_db: <- restaura el backup que copiamos a temp`

docker exec microservice_shop_project pg_restore -v --clean --no-owner --no-privileges -U jcvidal -d cardiocentro
/tmp/Municipio.csv

 ## More Details

## 💡 Detalles de los parámetros

**`microservice_shop_project`** → nombre del contenedor que ejecuta tu instancia de **PostgreSQL**.  
- **`jcvidal`** → tu usuario de Postgres (definido en POSTGRES_USER).

- microservice_shop_project → el nombre de tu contenedor de Postgres.
- shop_db → la base de datos destino (definida en POSTGRES_DB).
- /tmp/customers.backup → la ruta dentro del contenedor donde copiaste el archivo con docker cp.