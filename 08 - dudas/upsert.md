# que es sel upsert y en combinacion con quien se usa... con el set, push o ambos?

`upsert` es una función en MongoDB que combina las palabras "update" y "insert". Cuando se utiliza `upsert` en una operación de actualización, MongoDB intentará realizar una actualización. Si no encuentra ningún documento que coincida con los criterios de búsqueda especificados, insertará un nuevo documento con los datos proporcionados en la operación de actualización.

`upsert` se puede combinar con `$set`, `$push` o cualquier otro operador de actualización según la necesidad. La elección de usar `$set`, `$push` u otros operadores dependerá de lo que esté intentando lograr en su operación de actualización.

-   Con `$set`: Puede usar `upsert` junto con `$set` para actualizar un documento si existe o insertarlo si no existe. Por ejemplo, si desea actualizar un campo específico o agregarlo si el documento no existe, puede usar `$set` en combinación con `upsert`.

    ```json
    db.collection.updateOne(
      { _id: ObjectId("5f6c64d54b4db35d3d79b0bd") },
      { $set: { "campo": "nuevo_valor" } },
      { upsert: true }
    )
    ```

-   Con `$push`: Del mismo modo, puede usar `upsert` con `$push` para agregar un valor a un arreglo si el documento existe o insertarlo con el valor especificado si no existe.

    ```json
    db.collection.updateOne(
      { _id: ObjectId("5f6c64d54b4db35d3d79b0bd") },
      { $push: { "arreglo": "nuevo_valor" } },
      { upsert: true }
    )
    ```

En resumen, `upsert` se puede combinar con operadores de actualización como `$set`, `$push` o cualquier otro operador según la necesidad de actualizar o insertar documentos en una colección de MongoDB.
