# Agregaciones en Mongo



##### Nota:
##### Para hacer esta práctica se cargaron datos ficticios de empresas, denominado “productos.json” que se encontro en los recursos de esta actividad.

___
## Ejercicios

___
#### Cuenta los productos de tipo “medio”, usando un método básico 
```bash
MongoAgregaciones> db.Productos.countDocuments({ tipo: "medio" })

Total: 25
```

___
#### Indicar con un distinct, las empresas (fabricantes) que hay en la colección
```bash
MongoAgregaciones> db.Productos.distinct("fabricante")
[
  'A.O. Smith',
  'Alere',
  'American Tire Distributors Holdings',
  'Anthem',
  'Archrock',
  'Ascena Retail Group',
  'AutoNation',
  'Best Buy',
  'CIT Group',
  'Cabot',
  'Comcast',
  'Comerica',
  'Core-Mark Holding',
  'DST Systems',
  'Darling Ingredients',
  'Delta Air Lines',
  'Delta Tucker Holdings',
  "Dick's Sporting Goods",
  'First Solar',
  'HCA Holdings',
  'Hanesbrands',
  'Hartford Financial Services Group',
  'Hawaiian Holdings',
  'HealthSouth',
  'Hyatt Hotels',
  'Kar Auction Services',
  'Kelly Services',
  'Kemper',
  'Kimberly-Clark',
  'Lennar',
  'Mercury General',
  'Mondelez International',
  'Motorola Solutions',
  'Nasdaq OMX Group',
  'National Oilwell Varco',
  'Nordstrom',
  'OneMain Holdings',
  'Oneok',
  'Orbital ATK',
  'Pep Boys-Mann',
  'Pool',
  'Precision Castparts',
  'Primoris Services',
  'Raymond James Financial',
  'Seaboard',
  'Securian Financial Group',
  'Simon Property Group',
  'State Farm Insurance Cos.',
  'State Street Corp.',
  'SunPower',
  'TEGNA',
  'Telephone & Data Systems',
  'Total System Services',
  'Tractor Supply',
  'TransDigm Group',
  'Trinity Industries',
  'TrueBlue',
  'Universal American',
  'Universal Health Services',
  'WGL Holdings',
  "Wendy's",
  'Werner Enterprises',
  'WestRock',
  'Williams-Sonoma'
]
```
___
#### Usando aggregate, visualizar los productos que tengan más de 80 unidades 
```bash
MongoAgregaciones> db.Productos.aggregate([
...     { $match: { unidades: { $gt: 80 } } }
... ])
[
  {
    _id: ObjectId("6615be6c407bf3015982cef2"),
    codigo: 0,
    nombre: 'Fantastic Wooden Fish',
    unidades: 95,
    precio: 291,
    fabricante: 'Kimberly-Clark',
    tipo: 'avanzado'
  },
  {
    _id: ObjectId("6615be6c407bf3015982cef4"),
    codigo: 2,
    nombre: 'Small Soft Fish',
    unidades: 96,
    precio: 189,
    fabricante: 'Primoris Services',
    tipo: 'medio'
  },
  {
    _id: ObjectId("6615be6c407bf3015982cefe"),
    codigo: 12,
    nombre: 'Refined Concrete Salad',
    unidades: 90,
    precio: 129,
    fabricante: 'Universal Health Services',
    tipo: 'avanzado'
  },
  {
    _id: ObjectId("6615be6c407bf3015982cf10"),
    codigo: 30,
    nombre: 'Small Rubber Pants',
    unidades: 89,
    precio: 16,
    fabricante: 'Hanesbrands',
    tipo: 'basico'
  },
  {
    _id: ObjectId("6615be6c407bf3015982cf13"),
    codigo: 33,
    nombre: 'Generic Concrete Hat',
    unidades: 82,
    precio: 70,
    fabricante: 'American Tire Distributors Holdings',
    tipo: 'basico'
  },
  {
    _id: ObjectId("6615be6c407bf3015982cf27"),
    codigo: 53,
    nombre: 'Licensed Plastic Hat',
    unidades: 96,
    precio: 38,
    fabricante: 'Best Buy',
    tipo: 'medio'
  },
  {
    _id: ObjectId("6615be6c407bf3015982cf28"),
    codigo: 54,
    nombre: 'Generic Metal Sausages',
    unidades: 84,
    precio: 77,
    fabricante: 'DST Systems',
    tipo: 'medio'
  },
  {
    _id: ObjectId("6615be6c407bf3015982cf2f"),
    codigo: 61,
    nombre: 'Sleek Rubber Keyboard',
    unidades: 82,
    precio: 33,
    fabricante: 'Alere',
    tipo: 'basico'
  },
  {
    _id: ObjectId("6615be6c407bf3015982cf34"),
    codigo: 66,
    nombre: 'Incredible Concrete Fish',
    unidades: 96,
    precio: 336,
    fabricante: 'Darling Ingredients',
    tipo: 'medio'
  }
]
```

___
#### Con $project visualizar solo el nombre, unidades y precio de los productos que tengan menos de 10 unidades
```bash
MongoAgregaciones> db.Productos.aggregate([
...     { $match: { unidades: { $lt: 10 } } },
...     {
...         $project: {
...             _id: 0,
...             nombre: 1,
...             unidades: 1,
...             precio: 1
...         }
...     }
... ])
[
  { nombre: 'Ergonomic Metal Ball', unidades: 5, precio: 246 },
  { nombre: 'Handmade Plastic Hat', unidades: 7, precio: 253 },
  { nombre: 'Ergonomic Metal Table', unidades: 0, precio: 94 },
  { nombre: 'Practical Frozen Chips', unidades: 0, precio: 305 },
  { nombre: 'Fantastic Metal Pants', unidades: 5, precio: 129 },
  { nombre: 'Intelligent Frozen Sausages', unidades: 3, precio: 111 },
  { nombre: 'Rustic Plastic Mouse', unidades: 5, precio: 24 }
]
```
___
#### Con $project ponemos el fabricante, pero le cambiamos el nombre por “empresa”. Usamos el mismo comando anterior 
```bash
MongoAgregaciones> db.Productos.aggregate([
...     { $match: { unidades: { $lt: 10 } } },
...     {
...         $project: {
...             _id: 0,
...             nombre: 1,
...             unidades: 1,
...             precio: 1,
...             empresa: "$fabricante"
...         }
...     }
... ])
[
  {
    nombre: 'Ergonomic Metal Ball',
    unidades: 5,
    precio: 246,
    empresa: 'Seaboard'
  },
  {
    nombre: 'Handmade Plastic Hat',
    unidades: 7,
    precio: 253,
    empresa: "Dick's Sporting Goods"
  },
  {
    nombre: 'Ergonomic Metal Table',
    unidades: 0,
    precio: 94,
    empresa: 'Kelly Services'
  },
  {
    nombre: 'Practical Frozen Chips',
    unidades: 0,
    precio: 305,
    empresa: 'Delta Air Lines'
  },
  {
    nombre: 'Fantastic Metal Pants',
    unidades: 5,
    precio: 129,
    empresa: 'OneMain Holdings'
  },
  {
    nombre: 'Intelligent Frozen Sausages',
    unidades: 3,
    precio: 111,
    empresa: 'A.O. Smith'
  },
  {
    nombre: 'Rustic Plastic Mouse',
    unidades: 5,
    precio: 24,
    empresa: 'Orbital ATK'
  }
]


MongoAgregaciones> db.Productos.aggregate([
...     { $match: { unidades: { $lt: 10 } } },
...     {
...         $project: {
...             _id: 0,
...             nombre: 1,
...             unidades: 1,
...             precio: 1,
...             empresa: "$fabricante",
...             total: { $multiply: ["$precio", "$unidades"] }
...         }
...     }
... ])
[
  {
    nombre: 'Ergonomic Metal Ball',
    unidades: 5,
    precio: 246,
    empresa: 'Seaboard',
    total: 1230
  },
  {
    nombre: 'Handmade Plastic Hat',
    unidades: 7,
    precio: 253,
    empresa: "Dick's Sporting Goods",
    total: 1771
  },
  {
    nombre: 'Ergonomic Metal Table',
    unidades: 0,
    precio: 94,
    empresa: 'Kelly Services',
    total: 0
  },
  {
    nombre: 'Practical Frozen Chips',
    unidades: 0,
    precio: 305,
    empresa: 'Delta Air Lines',
    total: 0
  },
  {
    nombre: 'Fantastic Metal Pants',
    unidades: 5,
    precio: 129,
    empresa: 'OneMain Holdings',
    total: 645
  },
  {
    nombre: 'Intelligent Frozen Sausages',
    unidades: 3,
    precio: 111,
    empresa: 'A.O. Smith',
    total: 333
  },
  {
    nombre: 'Rustic Plastic Mouse',
    unidades: 5,
    precio: 24,
    empresa: 'Orbital ATK',
    total: 120
  }
]

```
___
#### Añadir a la consulta anterior un campo calculado que se llame total y que multiplique precio por unidades. 
```bash
MongoAgregaciones> db.Productos.aggregate([
...     { $match: { unidades: { $lt: 10 } } },
...     {
...         $project: {
...             _id: 0,
...             nombre: { $toUpper: "$nombre" },
...             unidades: 1,
...             precio: 1,
...             empresa: "$fabricante",
...             total: { $multiply: ["$precio", "$unidades"] }
...         }
...     }
... ])
[
  {
    nombre: 'ERGONOMIC METAL BALL',
    unidades: 5,
    precio: 246,
    empresa: 'Seaboard',
    total: 1230
  },
  {
    nombre: 'HANDMADE PLASTIC HAT',
    unidades: 7,
    precio: 253,
    empresa: "Dick's Sporting Goods",
    total: 1771
  },
  {
    nombre: 'ERGONOMIC METAL TABLE',
    unidades: 0,
    precio: 94,
    empresa: 'Kelly Services',
    total: 0
  },
  {
    nombre: 'PRACTICAL FROZEN CHIPS',
    unidades: 0,
    precio: 305,
    empresa: 'Delta Air Lines',
    total: 0
  },
  {
    nombre: 'FANTASTIC METAL PANTS',
    unidades: 5,
    precio: 129,
    empresa: 'OneMain Holdings',
    total: 645
  },
  {
    nombre: 'INTELLIGENT FROZEN SAUSAGES',
    unidades: 3,
    precio: 111,
    empresa: 'A.O. Smith',
    total: 333
  },
  {
    nombre: 'RUSTIC PLASTIC MOUSE',
    unidades: 5,
    precio: 24,
    empresa: 'Orbital ATK',
    total: 120
  }
]
```
___
#### Hacer que el nombre salga en mayúsculas con el operador $toUpper 
```bash
MongoAgregaciones> db.Productos.aggregate([
...     { $project: {
...         nombre: 1,
...         tipo: 1,
...         completo: { $concat: ["$nombre", " ", "$tipo"] }
...     } }
... ])
[
  {
    _id: ObjectId("6615be6c407bf3015982cef2"),
    nombre: 'Fantastic Wooden Fish',
    tipo: 'avanzado',
    completo: 'Fantastic Wooden Fish avanzado'
  },
  {
    _id: ObjectId("6615be6c407bf3015982cef3"),
    nombre: 'Rustic Concrete Pants',
    tipo: 'medio',
    completo: 'Rustic Concrete Pants medio'
  },
  {
    _id: ObjectId("6615be6c407bf3015982cef4"),
    nombre: 'Small Soft Fish',
    tipo: 'medio',
    completo: 'Small Soft Fish medio'
  },
  {
    _id: ObjectId("6615be6c407bf3015982cef5"),
    nombre: 'Practical Soft Pants',
    tipo: 'avanzado',
    completo: 'Practical Soft Pants avanzado'
  },
  {
    _id: ObjectId("6615be6c407bf3015982cef6"),
    nombre: 'Ergonomic Metal Ball',
    tipo: 'medio',
    completo: 'Ergonomic Metal Ball medio'
  },
  {
    _id: ObjectId("6615be6c407bf3015982cef7"),
    nombre: 'Small Steel Gloves',
    tipo: 'avanzado',
    completo: 'Small Steel Gloves avanzado'
  },
  {
    _id: ObjectId("6615be6c407bf3015982cef8"),
    nombre: 'Ergonomic Wooden Shirt',
    tipo: 'avanzado',
    completo: 'Ergonomic Wooden Shirt avanzado'
  },
  {
    _id: ObjectId("6615be6c407bf3015982cef9"),
    nombre: 'Handmade Steel Chair',
    tipo: 'medio',
    completo: 'Handmade Steel Chair medio'
  },
  {
    _id: ObjectId("6615be6c407bf3015982cefa"),
    nombre: 'Handcrafted Soft Gloves',
    tipo: 'basico',
    completo: 'Handcrafted Soft Gloves basico'
  },
  {
    _id: ObjectId("6615be6c407bf3015982cefb"),
    nombre: 'Fantastic Concrete Salad',
    tipo: 'basico',
    completo: 'Fantastic Concrete Salad basico'
  },
  {
    _id: ObjectId("6615be6c407bf3015982cefc"),
    nombre: 'Handmade Plastic Hat',
    tipo: 'medio',
    completo: 'Handmade Plastic Hat medio'
  },
  {
    _id: ObjectId("6615be6c407bf3015982cefd"),
    nombre: 'Refined Wooden Tuna',
    tipo: 'avanzado',
    completo: 'Refined Wooden Tuna avanzado'
  },
  {
    _id: ObjectId("6615be6c407bf3015982cefe"),
    nombre: 'Refined Concrete Salad',
    tipo: 'avanzado',
    completo: 'Refined Concrete Salad avanzado'
  },
  {
    _id: ObjectId("6615be6c407bf3015982ceff"),
    nombre: 'Unbranded Soft Fish',
    tipo: 'basico',
    completo: 'Unbranded Soft Fish basico'
  },
  {
    _id: ObjectId("6615be6c407bf3015982cf00"),
    nombre: 'Small Concrete Fish',
    tipo: 'medio',
    completo: 'Small Concrete Fish medio'
  },
  {
    _id: ObjectId("6615be6c407bf3015982cf01"),
    nombre: 'Refined Concrete Bike',
    tipo: 'basico',
    completo: 'Refined Concrete Bike basico'
  },
  {
    _id: ObjectId("6615be6c407bf3015982cf02"),
    nombre: 'Tasty Cotton Pants',
    tipo: 'basico',
    completo: 'Tasty Cotton Pants basico'
  },
  {
    _id: ObjectId("6615be6c407bf3015982cf03"),
    nombre: 'Incredible Granite Gloves',
    tipo: 'avanzado',
    completo: 'Incredible Granite Gloves avanzado'
  },
  {
    _id: ObjectId("6615be6c407bf3015982cf04"),
    nombre: 'Practical Metal Mouse',
    tipo: 'basico',
    completo: 'Practical Metal Mouse basico'
  },
  {
    _id: ObjectId("6615be6c407bf3015982cf05"),
    nombre: 'Handcrafted Steel Chicken',
    tipo: 'medio',
    completo: 'Handcrafted Steel Chicken medio'
  }
]

```
___
#### Añadir un campo calculado que ponga el nombre del producto y el tipo concatenado con el operador $concat. Le llamamos al campo “completo”
```bash
MongoAgregaciones> db.Productos.aggregate([
...     { $match: { unidades: { $lt: 10 } } },
...     { $project: {
...         nombre: 1,
...         unidades: 1,
...         precio: 1,
...         total: { $multiply: ["$unidades", "$precio"] }
...     } },
...     { $sort: { total: 1 } }
... ])
[
  {
    _id: ObjectId("6615be6c407bf3015982cf06"),
    nombre: 'Ergonomic Metal Table',
    unidades: 0,
    precio: 94,
    total: 0
  },
  {
    _id: ObjectId("6615be6c407bf3015982cf21"),
    nombre: 'Practical Frozen Chips',
    unidades: 0,
    precio: 305,
    total: 0
  },
  {
    _id: ObjectId("6615be6c407bf3015982cf31"),
    nombre: 'Rustic Plastic Mouse',
    unidades: 5,
    precio: 24,
    total: 120
  },
  {
    _id: ObjectId("6615be6c407bf3015982cf2a"),
    nombre: 'Intelligent Frozen Sausages',
    unidades: 3,
    precio: 111,
    total: 333
  },
  {
    _id: ObjectId("6615be6c407bf3015982cf25"),
    nombre: 'Fantastic Metal Pants',
    unidades: 5,
    precio: 129,
    total: 645
  },
  {
    _id: ObjectId("6615be6c407bf3015982cef6"),
    nombre: 'Ergonomic Metal Ball',
    unidades: 5,
    precio: 246,
    total: 1230
  },
  {
    _id: ObjectId("6615be6c407bf3015982cefc"),
    nombre: 'Handmade Plastic Hat',
    unidades: 7,
    precio: 253,
    total: 1771
  }
]
```
___
#### Ordena el resultado por el campo “total”
```bash
MongoAgregaciones> db.Productos.aggregate([
...     { $group: {
...         _id: "$tipo",
...         totalProductos: { $sum: 1 }
...     } }
... ])
[
  { _id: 'medio', totalProductos: 25 },
  { _id: 'basico', totalProductos: 24 },
  { _id: 'avanzado', totalProductos: 18 }
]
```
___
#### Haciendo una nueva consulta, averiguar el numero de productos por tipo de producto
```bash
MongoAgregaciones> db.Productos.aggregate([
...     { $group: {
...         _id: "$tipo",
...         totalProductos: { $sum: 1 }
...     } }
... ])
[
  { _id: 'medio', totalProductos: 25 },
  { _id: 'basico', totalProductos: 24 },
  { _id: 'avanzado', totalProductos: 18 }
]
```
___
#### Añadir el valor mayor y el menor
```bash
MongoAgregaciones> db.Productos.aggregate([
...     { $group: {
...         _id: "$tipo",
...         totalProductos: { $sum: 1 },
...         maxPrecio: { $max: "$precio" },
...         minPrecio: { $min: "$precio" }
...     } }
... ])
[
  {
    _id: 'avanzado',
    totalProductos: 18,
    maxPrecio: 331,
    minPrecio: 18
  },
  { _id: 'basico', totalProductos: 24, maxPrecio: 285, minPrecio: 16 },
  { _id: 'medio', totalProductos: 25, maxPrecio: 337, minPrecio: 16 }
]
```
___
#### Añade el total de unidades por cada tipo
```bash
MongoAgregaciones> db.Productos.aggregate([
...     { $group: {
...         _id: "$tipo",
...         totalUnidades: { $sum: "$unidades" }
...     } }
... ])
[
  { _id: 'medio', totalUnidades: 1224 },
  { _id: 'avanzado', totalUnidades: 773 },
  { _id: 'basico', totalUnidades: 1067 }
]
```
___
#### Con el operador $set y el operador “$substr” visualiza todos los datos del producto "Small Metal Tuna" y los primeros 5 caracteres del nombre
```bash
MongoAgregaciones> db.Productos.aggregate([
...     { $match: { nombre: "Small Metal Tuna" } },
...     { $set: {
...         primeros5Caracteres: { $substr: ["$nombre", 0, 5] }
...     } },
...     { $project: {
...         _id: 0,
...         nombre: 1,
...         tipo: 1,
...         unidades: 1,
...         precio: 1,
...         primeros5Caracteres: 1
...     } }
... ])
[
  {
    nombre: 'Small Metal Tuna',
    unidades: 46,
    precio: 43,
    tipo: 'medio',
    primeros5Caracteres: 'Small'
  }
]
```
___
#### Creamos una salida que tenga el nombre del articulo y el total (precio por unidades) y lo guardamos en una colección denominada productos2
```bash
MongoAgregaciones> db.Productos.aggregate([
...     {
...         $project: {
...             _id: 0,
...             nombre: 1,
...             total: { $multiply: ["$precio", "$unidades"] }
...         }
...     },
...     {
...         $out: "productos2"
...     }
... ])
```
___
#### Creamos una salida que tenga el nombre del articulo y el total (precio por unidades) y lo guardamos en una colección denominada productos2
```bash
MongoAgregaciones> db.Productos.aggregate([
...     {
...         $project: {
...             _id: 0,
...             nombre: 1,
...             total: { $multiply: ["$precio", "$unidades"] }
...         }
...     },
...     {
...         $out: "productos2"
...     }
... ])
```
___
#### Comprobamos que se ha creado
```bash
MongoAgregaciones> show collections
Productos
productos2
```
___
#### Hacemos un find para comprobar el resultado
```bash
MongoAgregaciones> db.productos2.find()
[
  {
    _id: ObjectId("6615cee33e20a1540c6b1700"),
    nombre: 'Fantastic Wooden Fish',
    total: 27645
  },
  {
    _id: ObjectId("6615cee33e20a1540c6b1701"),
    nombre: 'Rustic Concrete Pants',
    total: 18414
  },
  {
    _id: ObjectId("6615cee33e20a1540c6b1702"),
    nombre: 'Small Soft Fish',
    total: 18144
  },
  {
    _id: ObjectId("6615cee33e20a1540c6b1703"),
    nombre: 'Practical Soft Pants',
    total: 2747
  },
  {
    _id: ObjectId("6615cee33e20a1540c6b1704"),
    nombre: 'Ergonomic Metal Ball',
    total: 1230
  },
  {
    _id: ObjectId("6615cee33e20a1540c6b1705"),
    nombre: 'Small Steel Gloves',
    total: 1665
  },
  {
    _id: ObjectId("6615cee33e20a1540c6b1706"),
    nombre: 'Ergonomic Wooden Shirt',
    total: 14994
  },
  {
    _id: ObjectId("6615cee33e20a1540c6b1707"),
    nombre: 'Handmade Steel Chair',
    total: 5392
  },
  {
    _id: ObjectId("6615cee33e20a1540c6b1708"),
    nombre: 'Handcrafted Soft Gloves',
    total: 4512
  },
  {
    _id: ObjectId("6615cee33e20a1540c6b1709"),
    nombre: 'Fantastic Concrete Salad',
    total: 12985
  },
  {
    _id: ObjectId("6615cee33e20a1540c6b170a"),
    nombre: 'Handmade Plastic Hat',
    total: 1771
  },
  {
    _id: ObjectId("6615cee33e20a1540c6b170b"),
    nombre: 'Refined Wooden Tuna',
    total: 8480
  },
  {
    _id: ObjectId("6615cee33e20a1540c6b170c"),
    nombre: 'Refined Concrete Salad',
    total: 11610
  },
  {
    _id: ObjectId("6615cee33e20a1540c6b170d"),
    nombre: 'Unbranded Soft Fish',
    total: 9434
  },
  {
    _id: ObjectId("6615cee33e20a1540c6b170e"),
    nombre: 'Small Concrete Fish',
    total: 5040
  },
  {
    _id: ObjectId("6615cee33e20a1540c6b170f"),
    nombre: 'Refined Concrete Bike',
    total: 2700
  },
  {
    _id: ObjectId("6615cee33e20a1540c6b1710"),
    nombre: 'Tasty Cotton Pants',
    total: 3536
  },
  {
    _id: ObjectId("6615cee33e20a1540c6b1711"),
    nombre: 'Incredible Granite Gloves',
    total: 20590
  },
  {
    _id: ObjectId("6615cee33e20a1540c6b1712"),
    nombre: 'Practical Metal Mouse',
    total: 6650
  },
  {
    _id: ObjectId("6615cee33e20a1540c6b1713"),
    nombre: 'Handcrafted Steel Chicken',
    total: 6215
  }
]
```