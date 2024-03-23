# Prácticas de MongoDB

## Primeros Comandos

### Iniciar el contenedor de Docker y acceder a la base de datos:
```bash
docker start example-mongo
docker exec -it example-mongo bash
mongosh

```
___
## Ejecutar comandos basicos

```
use practica1
show dbs
show collections
db.createCollection("facturas")
db.facturas.insertOne({
    Cod_factura: 10,
    Cliente: "Frutas Ramirez",
    Total: 223
})
db.facturas.find()
db.productos.insertOne({
    Cod_producto: 2,
    Nombre: "Martillo X2",
    Precio: 20,
    Unidades: 50,
    Fabricantes: ["Fab1", "Fab2", "Fab3", "Fab4"]
})
db.fabricantes.insertOne({
    _id: 1,
    Nombre: "Fab1",
    Localidad: { Ciudad: "Buenos Aires", Pais: "Argentina", Calle: "Calle Pez 27", Cod: 29000 }
})
```
___
## Borrar documentos 
```
db.facturas.deleteOne({ Cliente: "Frutas Ramirez" })

```
___
## Borrar documentos con un total mayor de 200
```
db.facturas.deleteMany({ Total: { $gt: 200 } })

```
___
## Practica de Mongo con find
___
##### Entrar al docker de mongo
![Entrar al docker de mongo](./Images/img1.png)
___
##### Creacion de una Base de datos denominada practica1
![Base de datos](./Images/img2.png)
___
##### Usar el show dbs
![Show dbs](./Images/img3.png)
___
##### Creacion de una colección denominada “facturas” 
![Base de datos](./Images/img4.png)
___
##### Insertar un documento 
![Base de datos](./Images/img5.png)
___
##### comprobar que se realizo la insercion
![Base de datos](./Images/img6.png)
___
##### Insertar una nueva fila en facturas y comprobar con un “find”
![Base de datos](./Images/img7.png)
___
##### Insertamos un documento en una colección que no existe (productos)  
![Base de datos](./Images/img8.png)
___
##### Hacemos un “show collections” para comprobar que se ha creado 
![Base de datos](./Images/img9.png)
___

![Base de datos](./Images/img10.png)
___
##### Hacemos un find() para comprobar el resultado y comprobamos que se ha borrado
![Base de datos](./Images/img11.png)
___
##### insertar un documento en una colección denominada “fabricantes” para probar los subdocumentos y la clave “_id” personalizada.

![Base de datos](./Images/img12.png)
___
##### Comprobar con el find que se han creado
![Base de datos](./Images/img13.png)
___
## Consultas
##### Importacion de la base de datos "empleados"
![Base de datos](./Images/img14.png)
___
##### Usar find para comprobar que se importo correctamente
![Base de datos](./Images/img15.png)
___
* Buscar todas los empleados que trabajen en Google
![Base de datos](./Images/img16.png)
___
* Empleados que vivan en Peru
![Base de datos](./Images/img17.png)
___
* Empleados que ganen más de 8.000 dolares
![Base de datos](./Images/img18.png)
___
* Empleados con ventas inferiores a 10000
![Base de datos](./Images/img19.png)
___
* Hacer el comando anterior pero devolviendo una sola fila
![Base de datos](./Images/img20.png)
___
* Empleados que trabajen en Google o en Yahoo con el operador $or
![Base de datos](./Images/img21.png)
___
* Empleados que trabajen en Google o en Yahoo con el operador $in 
![Base de datos](./Images/img22.png)
___
* Empleados de Amazon que ganen mas de 9000 dolares
![Base de datos](./Images/img23.png)
___
* Empleados que trabajen en Yahoo que ganen mas de 6000 o empleados que trabajen en Google que tangan ventas inferiores a 20000

![Base de datos](./Images/img24.png)
___
* Visualizar el nombre, apellidos y el país de cada empleado
![Base de datos](./Images/img25.png)

```

```
___
## Practica de Mongo con agregaciones

#### pipeline 1:
```
db.getCollection('libros').aggregate(
  [
    { $match: { editorial: 'Biblio' } },
    {
      $project: {
        _id: 0,
        titulo: 1,
        precio: 1,
        cantidad: 1,
        'Nombre Editorial': '$editorial',
        'Total ganancia': {
          $multiply: ['$precio', '$cantidad']
        }
      }
    },
    { $sort: { precio: 1 } }
  ],
  { maxTimeMS: 60000, allowDiskUse: true }
);

```
___
#### pipeline 2:
```
db.getCollection('libros').aggregate(
  [
    {
      $group: {
        _id: '$editorial',
        'Numero documentos': { $count: {} }
      }
    }
  ],
  { maxTimeMS: 60000, allowDiskUse: true }
);

```
___
#### pipeline 3:
```
db.getCollection('libros').aggregate(
  [
    {
      $group: {
        _id: '$editorial',
        'Numero de docs': { $count: {} },
        media: { $avg: '$precio' }
      }
    },
    {
      $group: {
        _id: '$editorial',
        'Numero de docs': { $count: {} },
        media: { $avg: '$precio' },
        'Precio maximo': { $max: '$precio' }
      }
    },
    { $sort: { 'Precio maximo': 1 } },
    {
      $group: {
        _id: 'editorial',
        Numero: { $count: {} },
        media: { $avg: '$precio' }
      }
    },
    {
      $set: {
        'Media total': { $trunc: '$media' }
      }
    }
  ],
  { maxTimeMS: 60000, allowDiskUse: true }
);

```
___
#### pipeline 4:
```
db.getCollection('libros').aggregate(
  [{ $unset: 'Media_editoriales' }],
  { maxTimeMS: 60000, allowDiskUse: true }
);

