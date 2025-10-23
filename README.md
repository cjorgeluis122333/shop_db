# Realizar un Backup de una base de datos
Para realizar un backup de una base de datos teniendo en cuenta las distintas situaciones que se puedan plantear.

## Para versiones compatibles con los backup de posgre
Caso se esto son los .backup y los .sql

### Paso 1: Copiar el restore a una ruta dentro del el mismo projecto:
EJEMPLO:
 docker cp "Absolute Path" <container_name>:/tmp/customers.backup

```docker
  docker cp "C:\Users\cjorg\Downloads\4to Año\Sistemas de la Informacion\Tare Extraclase practica\Fuentes de datos\7- Cardiocentro\Municipio.csv" microservice_shop_project:/tmp/Municipio.csv
```

### Paso 2: Restaurar la base de datos
dentro de shop_db: <- restaura el backup que copiamos a temp
```docker
docker exec microservice_shop_project pg_restore -v --clean --no-owner --no-privileges -U jcvidal -d cardiocentro
/tmp/Municipio.csv
```
    ### More Details
#microservice_shop_project → el nombre de tu contenedor de Postgres.
#jcvidal → tu usuario de Postgres (definido en POSTGRES_USER).
#shop_db → la base de datos destino (definida en POSTGRES_DB).
#/tmp/customers.backup → la ruta dentro del contenedor donde copiaste el archivo con docker cp.