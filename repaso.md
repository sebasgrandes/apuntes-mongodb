# Repaso EXAMEN

-   $lookup: tipo join de colecciones
-   $out: retorna los resultados en una nueva coleccion
-   indexes multiples: equality, sort, range

-   $elemMatch: usado para especificar que estoy tratando con un array
-   findAndModify: actualiza y devuelve un solo documento. util cuando deseo realizar una operacion de lectura y escritura en un solo paso y quiero asegurarme de que otras operaciones no interfieran en la lectura y escritura.

-   tipos de datos admitidos en bson: double, string, object, array, binary data (imagenes o archivos), undefined, objectid, boolean,d ate, null, regex, js, dbpointer (puntero a otro documento), symbol, 32 y 64 bit integer (numeros enteros de 32 y 64 bits), timestamp decimal 128, min y max key.

-   bulkWrite(): metodo que permite realizar operaciones de escritura masiva en una coleccion, por ejemplo:

```javascript
const operations = [
    { insertOne: { document: { _id: 1, name: "John" } } },
    { updateOne: { filter: { _id: 2 }, update: { $set: { name: "Alice" } } } },
    { deleteOne: { filter: { _id: 3 } } },
];
db.collection.bulkWrite(operations);
```

**¿puedo insertar un id duplicado?**: Si estás tratando de insertar los documentos A y C en una colección donde los \_id ya existen, tendrás un problema porque MongoDB no permite la inserción de documentos con \_id duplicados en una colección. Cada documento debe tener un \_id único dentro de la colección. Psdt.: Puedes insertar un documento con id 0

**que pasa si en updateone y replace one, en el primer parametro no le coloco nada**

-   La operación replaceOne({}, {a: "ten", b: "five"}) reemplaza el primer documento que coincida con el filtro {}, que es un documento vacío. Esto significa que se actualizará solo el primer documento en la colección, y los otros documentos permanecerán sin cambios.

Sucede lo mismo que con replaceOne...

**que pasa si en updateManu, en el primer parametro no le coloco nada**

-   Con updateMany({}, ...): Especifique un documento vacío { } (en el primer parametro) para actualizar todos los documentos de la colección.

**tokenization**

-   Tokenización mediante 'regexCaptureGroup':Se utiliza cuando se necesitan patrones específicos definidos por expresiones regulares para dividir el texto en tokens.
-   Tokenización mediante 'edgeGram':Se utiliza para crear índices de autocompletado que buscan coincidencias al comienzo de una palabra.
-   Tokenización mediante 'nGram':Se utiliza para buscar coincidencias en general dentro del texto, sin un enfoque específico en el inicio de una palabra.
-   Tokenización mediante 'matchNGram':Se utiliza para buscar gramas específicos dentro de una palabra, pero su uso específico puede variar según la implementación.

**busqueda en texto completo**
La diferencia principal es que la Consulta 1 se utiliza cuando se trabaja con índices de búsqueda de texto completo y se espera que el campo contenga texto estructurado, mientras que la Consulta 2 se utiliza para buscar coincidencias directamente en un campo específico sin necesidad de un índice de búsqueda de texto completo.

```javascript
// consulta 1
db.restaurants.aggregate([
    {
        $search: {
            text: { path: "name", query: "cuban" },
        },
    },
]);
// consulta 2
db.restaurants.aggregate([
    {
        $search: {
            name: "cuban",
        },
    },
]);
```

**¿si hago un getindexes que se muestra en pantalla?**

```javascript
{ v: 2, key: { price: 1, product: 1 }, name: 'price_1_product_1' }
```

**estructura de un pipeline en mongo db**  
el project puede entrar en cualquier lugar despues del $match

```javascript
db.orders.aggregate([
    { $match: { status: "A" } },
    { $group: { _id: "$cust_id", total: { $sum: "$amount" } } },
    { $sort: { total: -1 } },
    { $limit: 2 },
]);
```

**diferencia entre deleteone y remove**... "delete" así solo no existe... remove esta deprecado

```javascript
// Eliminar un producto específico por su _id
db.products.deleteOne({ _id: ObjectId("609f925855e94d2b7c2c49ae") });

// Eliminar todos los productos con un precio menor a $10
// db.products.remove({ price: { $lt: 10 } });
```

**READ**

-   partes del .find: filtro, proyeccion y operaciones (sort, limit, skip, etc.)

```javascript

db.usuarios.find(
// Filtro
{ edad: { $gte: 18 } },

// Proyección
{ nombre: 1, email: 1, \_id: 0 },

// Opciones
{ sort: { edad: 1 }, limit: 10 }
);

```

-   skip(): se usa para omitir un número específico de documentos al realizar una consulta.

```javascript
// Consulta para obtener la segunda página de resultados, mostrando 10 documentos por página
db.usuarios.find().skip(10).limit(10);
// En este ejemplo, skip(10) se utiliza para omitir los primeros 10 documentos que coincidan con la consulta, y limit(10) se utiliza para limitar el número de documentos devueltos a 10, lo que resulta en la segunda página de resultados, mostrando 10 documentos por página.
```

**UPDATE**

-   updateOne(): se utiliza para **actualizar** un solo documento que cumple con los criterios de filtro especificados. Este método te permite realizar modificaciones en un documento existente de manera eficiente sin necesidad de recuperar el documento completo.

```javascript
db.collection.updateOne(
    { filtro: valor }, // Criterio de búsqueda
    { $set: { campo: nuevoValor } } // Operación de actualización
);
```

-   findOneAndUpdate(): Este método **actualiza y devuelve un solo documento que cumple con los criterios de filtrado especificados**. Puedes proporcionar una serie de operaciones de actualización (como $set, $unset, etc.) para modificar el documento seleccionado. Es útil cuando deseas actualizar un documento existente y obtener el documento original antes de la modificación o el documento modificado después de la modificación.

```javascript
db.collection.findOneAndUpdate(
    { filtro: valor }, // Criterio de búsqueda
    { $set: { campo: nuevoValor } }, // Operación de actualización
    { returnOriginal: false } // Opción para devolver el documento modificado
);
```

-   replaceOne: Este método **reemplaza** un único documento que cumple con los criterios de filtrado especificados por otro documento proporcionado. Si existe un documento que coincide con los criterios de filtrado, se reemplaza completamente por el documento proporcionado. Si no hay coincidencias, no se realiza ninguna acción. Es útil cuando deseas reemplazar completamente un documento existente con otro documento.

```javascript
db.collection.replaceOne(
    { filtro: valor }, // Criterio de búsqueda
    { nuevoDocumento } // Nuevo documento
);
```

-   findOneAndReplace(): Este método _busca un solo documento que cumple con los criterios de filtrado especificados y lo reemplaza completamente por otro documento proporcionado_. **A diferencia de replaceOne, este método devuelve el documento original antes de la modificación**, lo que te permite ver qué documento fue reemplazado. Es útil cuando deseas reemplazar un documento existente y obtener el documento original antes de la modificación.

```javascript
db.collection.findOneAndReplace(
    { filtro: valor }, // Criterio de búsqueda
    { nuevoDocumento } // Nuevo documento
);
```

-   findOneAndDelete(): encuentra un documento que coincida con ciertos criterios de búsqueda y eliminarlo de la colección, todo en una sola operación atómica. ademas devuelve el documento eliminado.

-   $unset: En MongoDB, $unset es un operador de actualización que **se utiliza para eliminar un campo específico de un documento**. Esto significa que puedes utilizar $unset para eliminar un campo y su valor de un documento existente en una colección.

```javascript
db.usuarios.updateOne(
    { _id: 1 }, // Filtro para seleccionar el documento
    { $unset: { email: 1 } } // Operación de actualización para eliminar el campo "email"
);
```

-   upsert: inserta un elemento si no se encuentra... se puede usar junto a updateone, updatemany, replaceone, etc.

**DELETE**

-   db.collection.remove(): método en MongoDB que se utiliza para eliminar uno o varios documentos de una colección. Este método puede eliminar documentos basados en un criterio de filtro específico o eliminar todos los documentos de una colección si no se proporciona ningún filtro.

```javascript
// eliminando un solo documento
db.usuarios.remove(
    { _id: ObjectId("5f4565a4b153ca3d5361c093") } // Eliminar el documento con el ID especificado
);

// eliminando todos los documentos
db.usuarios.remove({});
```

-   drop(): Para eliminar una colección completa en MongoDB, puedes utilizar el método drop() en el objeto de la colección. Este método elimina la colección completa, incluidos todos sus índices y datos.

```javascript
// Por ejemplo, si quieres eliminar la colección usuarios, puedes hacerlo de la siguiente manera:
db.usuarios.drop();
```
