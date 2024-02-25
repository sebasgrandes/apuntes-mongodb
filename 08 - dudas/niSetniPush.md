# que sucede o que opcion se aplica por defecto si en un updateOne no utilizo el $set ni el $push

En MongoDB, si en un `updateOne()` no utilizas ni `$set` ni `$push`, se aplicará un comportamiento predeterminado que depende de la naturaleza del campo que estás intentando actualizar y del tipo de operación que estás realizando.

1. **Actualización de un campo existente**: Si el campo que intentas actualizar ya existe en el documento, pero no utilizas `$set` ni `$push`, entonces el valor del campo será reemplazado por completo con el nuevo valor proporcionado en la operación de actualización. Esto significa que el campo existente se sobrescribirá con el nuevo valor.

2. **Creación de un campo nuevo**: Si el campo que intentas actualizar no existe en el documento y no utilizas `$set` ni `$push`, entonces el comportamiento dependerá de si estás usando un operador de actualización (por ejemplo, `$inc`, `$mul`, etc.) o simplemente estás pasando un nuevo valor directamente. Si estás usando un operador de actualización, como `$inc` o `$mul`, el campo se creará con el valor proporcionado por ese operador. Si simplemente estás pasando un nuevo valor sin ningún operador, el campo no se creará en el documento.

Por ejemplo, si tienes un documento como este:

```json
{
    "_id": 1,
    "nombre": "Juan",
    "edad": 30
}
```

Y ejecutas un `updateOne()` sin `$set` ni `$push`:

```javascript
db.collection.updateOne({ _id: 1 }, { nombre: "Pedro" });
```

El documento se verá así después de la actualización:

```json
{
    "_id": 1,
    "nombre": "Pedro"
}
```

Observe que el campo `"edad"` desapareció porque no se especificó en la operación de actualización. Si deseas mantener los campos no especificados, necesitarías usar `$set` para actualizar solo los campos específicos que deseas modificar.
