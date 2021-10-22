# Herramientas de backup

## ¿Dónde conseguir las herramientas?

Disponibles en https://www.mongodb.com/try/download/database-tools

Es recomendables ubicar en la misma carpeta de los binarios de mongod, mongos, mongo, etc... para
así aprovechar la ruta en las variables de entorno

C:\Program Files\MongoDB\Server\4.4\bin

## mongodump

Realiza backup a nivel de base de datos o colección

Desde cualquier terminal

mongodump

--host <dirección servidor> | --uri <uri> (si no se indica el valor por defecto es localhost)

--port <puerto servidor>

--db=<base-de-datos>

--collection=<colección>

--out=<ruta>

Prueba

mongodump --port 27017 --db=clinica --out="backup/bck260721"

## mongorestore

Restaura un backup a nivel de base de datos o coleccion

mongorestore --port 27017 backup/bck260721 // se le pasa la ruta donde están los ficheros que devolvió mongodump

# Herramientas de importación/exportación de documentos MongoDB

## ¿Dónde conseguir las herramientas?

Disponibles en https://www.mongodb.com/try/download/database-tools

Es recomendables ubicar en la misma carpeta de los binarios de mongod, mongos, mongo, etc... para
así aprovechar la ruta en las variables de entorno

C:\Program Files\MongoDB\Server\4.4\bin

## mongoexport

Exporta los documentos en formato json (por defecto) ó csv

Se ejecuta con el comando del mismo de la herramienta seguido de las
siguiente opciones

mongoexport 

--host <dirección servidor> | --uri <uri> (si no se indica el valor por defecto es localhost)

--port <puerto servidor>

--db=<base-de-datos>

--collection=<colección>

--fields=<campo1,campo2,campo3, ...>

--type=csv

--noHeaderLine

--out=<ruta>/<nombrearchivo>.json | .csv

Desde cualquier terminal

mongoexport --port 27017 --db=maraton --collection=runners --fields=name,surname1,age --out=runners.json
mongoexport --port 27017 --db=maraton --collection=runners --fields=name,surname1,age --type=csv --out=runners.csv

## mongoimport

Importar documentos desde archivos json, csv ó tsv

mongoimport

--host <dirección servidor> | --uri <uri> (si no se indica el valor por defecto es localhost)

--port <puerto servidor>

--db=<base-de-datos>

--collection=<colección>

--file=<ruta>/<nombrearchivo>.json | .csv | .tsv

--type=<json | csv | tsv>

--mode=<insert | upsert | merge> // por defecto es insert

--upsertFields=<campo1, campo2, ...>

Creamos como ejemplo un archivo clientes.json con unos registros

{"_id": 1, "nombre": "Juan"}
{"_id": 2, "nombre": "Sara"}
{"_id": 3, "nombre": "Carlos"}
{"_id": 4, "nombre": "Raquel"}
{"_id": 5, "nombre": "Luis"}

mongoimport --port 27017 --db="shop10" --collection="customers" --file="customers.json" --type=json

Con el modo upsert actualiza los documentos coincidentes (para la coincidencia usa el campo _id) y los que no
encuentra los inserta como nuevos documentos

{"_id": 1, "nombre": "Pepe"}
{"_id": 2, "nombre": "Pepe"}
{"_id": 3, "nombre": "Pepe"}
{"_id": 4, "nombre": "Pepe"}
{"_id": 5, "nombre": "Pepe"}
{"_id": 10, "nombre": "Pepe"} // Este será creado

Si repetimos el comando anterior dara error por duplicidad de _id pero si añadimos el modo upsert actualizará e insertará:

mongoimport --port 27017 --db="shop10" --collection="customers" --file="customers.json" --type=json --mode=upsert

Con el modo merge en los documentos que ya existen (de nuevo _id es el campo de referencia), fusiona el documento entrante 
con el existente y en los nuevos realiza una inserción

{"_id": 1, "apellidos": "López"} // Fusiona
{"_id": 2, "apellidos": "López"}
{"_id": 3, "apellidos": "López"}
{"_id": 4, "apellidos": "López"}
{"_id": 5, "apellidos": "López"}
{"_id": 11, "apellidos": "López"} // Inserta como nuevo doc

mongoimport --port 27017 --db="shop10" --collection="customers" --file="customers.json" --type=json --mode=merge
