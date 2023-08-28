# Operaciones CRUD

## Escritura (insert)
### `insertOne()`

El método `insertOne()` te permite insertar un solo documento en una colección. Aquí tienes un ejemplo:

```javascript
db.miColeccion.insertOne({
  nombre: "Juan",
  edad: 30,
  ocupacion: "Desarrollador"
});
```

### `insertMany()`

El método `insertMany()` te permite insertar múltiples documentos en una colección. Aquí tienes un ejemplo:

```javascript
db.miColeccion.insertMany([
  {
    nombre: "María",
    edad: 25,
    ocupacion: "Diseñadora"
  },
  {
    nombre: "Carlos",
    edad: 28,
    ocupacion: "Analista"
  }
]);
```

## Lectura (find)

### `find()`

El método `find()` te permite recuperar documentos de una colección en función de ciertos criterios. Aquí tienes un ejemplo:

```javascript
// Recuperar todos los documentos con edad mayor a 25
db.miColeccion.find({ edad: { $gt: 25 } });
```

## Actualización

### `updateOne()`

El método `updateOne()` te permite actualizar un único documento en función de un filtro. Aquí tienes un ejemplo:

```javascript
// Actualizar el trabajo de Juan a "Ingeniero"
db.miColeccion.updateOne(
  { nombre: "Juan" },
  { $set: { ocupacion: "Ingeniero" } }
);
```

### `updateMany()`

El método `updateMany()` te permite actualizar varios documentos en función de un filtro. Aquí tienes un ejemplo:

```javascript
// Incrementar la edad de todos los desarrolladores en 1
db.miColeccion.updateMany(
  { ocupacion: "Desarrollador" },
  { $inc: { edad: 1 } }
);
```

## Eliminación

### `deleteOne()`

El método `deleteOne()` te permite eliminar un único documento en función de un filtro. Aquí tienes un ejemplo:

```javascript
// Eliminar el documento de María
db.miColeccion.deleteOne({ nombre: "María" });
```

### `deleteMany()`

El método `deleteMany()` te permite eliminar varios documentos en función de un filtro. Aquí tienes un ejemplo:

```javascript
// Eliminar todos los documentos con edad menor a 30
db.miColeccion.deleteMany({ edad: { $lt: 30 } });
```

## Escritura en Lote (Bulk Write)

MongoDB también admite operaciones de escritura en lote, lo que te permite realizar múltiples operaciones de inserción, actualización o eliminación en una sola llamada. Aquí tienes un ejemplo de una operación de escritura en lote que inserta varios documentos:

```javascript
db.miColeccion.bulkWrite([
  { insertOne: { document: { nombre: "Ana", edad: 22 } } },
  { insertOne: { document: { nombre: "Pedro", edad: 35 } } },
  // Otras operaciones aquí
]);
```

Recuerda que estas operaciones te permiten interactuar con tu base de datos MongoDB de manera eficiente y flexible, proporcionando diversas formas de crear, leer, actualizar y eliminar documentos.






# Agregaciones en MongoDB

Es una herramienta para procesar y analizar datos de manera avanzada. Permite realizar cálculos complejos, operaciones en múltiples documentos y transformaciones personalizadas. Aquí se presentan algunos conceptos clave y etapas dentro de la tubería de agregación:

## Fundamentos de la Agregación

Las agregaciones se utilizan para procesar y analizar datos de manera avanzada. Ofrecen varias ventajas:

- **Flexibilidad:** Las agregaciones permiten transformaciones y cálculos personalizados, útiles cuando las consultas simples no son suficientes.
- **Análisis Profundo:** Las agregaciones brindan un análisis detallado, lo que proporciona información detallada para tomar decisiones informadas y estratégicas.
- **Eficiencia:** Si bien las consultas básicas manejan operaciones simples, las agregaciones procesan de manera eficiente conjuntos de datos grandes y cálculos complejos en un solo paso.

## Etapas 

Las agregaciones constan de etapas que procesan documentos de manera secuencial. Cada etapa realiza una operación específica en los documentos y pasa los resultados a la siguiente etapa.

### `$match`

Filtra documentos en función de condiciones específicas. Ejemplo:

```javascript
db.ventas.aggregate([
  {
    $match: {
      fecha: { $gte: ISODate("2023-01-01"), $lt: ISODate("2023-03-01") },
      cantidad: { $gt: 10 }
    }
  }
]);
```

Esta etapa filtra documentos dentro de la colección "ventas" según un rango de fechas y cantidad.

### `$group`

Agrupa documentos en función de campos específicos y calcula valores agregados como sumas y promedios. Ejemplo:

```javascript
db.ventas.aggregate([
  {
    $group: {
      _id: "$categoria",
      promedioVentas: { $avg: "$precio" },
      totalVentas: { $sum: "$cantidad" }
    }
  }
]);
```

Esta etapa agrupa ventas por categoría y calcula ventas promedio (`$avg`) y ventas totales (`$sum`).

### `$lookup`

Realiza "joins" entre colecciones, combinando datos de manera eficiente. Ejemplo:

```javascript
db.ventas.aggregate([
  {
    $lookup: {
      from: "productos",
      localField: "productoId",
      foreignField: "_id",
      as: "producto"
    }
  }
]);
```

Esta etapa relaciona la colección "ventas" con la colección "productos" utilizando el campo `productoId`.

### `$project`

Especifica los campos para incluir o excluir en la salida. Ejemplo:

```javascript
db.ventas.aggregate([
  {
    $project: {
      _id: 0,
      name: 1,
      totalVentas: { $multiply: ["$cantidad", "$precio_unitario"] },
      year: { $year: "$fecha" }
    }
  }
]);
```

Esta etapa excluye `_id`, incluye `name`, calcula las ventas totales usando `$multiply` y extrae el año de la fecha.

### `$unwind`

Descompone un campo de tipo array para operaciones adicionales. Ejemplo:

```javascript
db.ventas.aggregate([
  {
    $unwind: "$quantities"
  },
  {
    $group: {
      _id: "$product",
      avgQuantity: { $avg: "$quantities" }
    }
  }
]);
```

Esta etapa descompone el array `quantities` y calcula el promedio.

### `$sort`

Ordena documentos en función de campos especificados. Ejemplo:

```javascript
db.users.aggregate([
  {
    $sort: {
      age: 1,
      quantityPost: -1
    }
  }
]);
```

Esta etapa ordena usuarios por edad de manera ascendente y por la cantidad de publicaciones de manera descendente.

## Autor
### Angel David Velasco

