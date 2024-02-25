## LECCIÓN 1: REEMPLAZO DE UN DOCUMENTO EN MONGODB

reemplazando un objeto: `db.collection.replaceOne(filter, replacement, options)`  
options es opcional y el filter consta de un campo y su valor, el cual determinará cual dato o documento debe ser reemplazado

ejemplo

```javascript
db.birds.replaceOne(
    { _id: ObjectId("6286809e2f3fa87b7d86dccd") },
    {
        common_name: "Morning Dove",
        scientific_name: "Zenaida macroura",
        wingspan_cm: 37.23,
        habitat: ["urban areas", "farms", "grassland"],
        diet: ["seeds"],
    }
);
```

## LECCIÓN 2: ACTUALIZACIÓN DE DOCUMENTOS MONGODB MEDIANTE UPDATEONE()

actualizando un documento: `updateOne()`  
ejemplo: db.collection.updateOne(<filter>, <update>, {options})  
options es opcional

podemos adicionalmente usar:

-   $set: para añadir nuevos campos o valores a un documento O reemplaza un valor de un campo por otro valor... si ese campo ya existía.
-   $push: añade elementos a un arreglo o array... o si el campo esta ausente, añade el campo de array con el valor como su elemento

junto con el $set podemos usar upsert en options si es que el updateone no da un match...

-   upsert: nos permite insertar documentos con la informacion proporcionada si es que el matcheo no existe (o sea, si es que no existen documentos coincidentes)

hay muchos más a parte de $set y $push, en la documentacion de mongodb.

asi se añade multiples valores a un campo... usa el each junto con el push. push por defecto solo añade un solo vaor creo, con each añades varios.

```javascript
db.birds.updateOne(
{ \_id: ObjectId("6268471e613e55b82d7065d7") }, { $push: { diet: { $each: ["newts", "opossum", "skunks", "squirrels"] } }}
)
```

## **DEPRECADO** findAndModify fue reemplazado por updateOneAndUpdate()

## LECCIÓN 3: ACTUALIZACIÓN DE DOCUMENTOS DE MONGODB MEDIANTE FINDANDMODIFY()

findAndModify(): se utiliza para devolver el documento que se acaba de actualizar

el metodo es como una combinacion dentre updateOne() y findOne(). acepta igual 3 parametros:

```javascript
// en options colocas: new: true // gracias al new true se devuelve el documento actualizado
db.<collection>.findAndModify({
    {query: <query o filter>, update: <update>}, <options>
})

// otro ejemplo correcto
db.birds.findAndModify({
  query: { common_name: "Blue Jay" },
  update: { $inc: { sightings_count: 1 }},
  new: true,
})
```

## LECCIÓN 4: ACTUALIZACIÓN DE DOCUMENTOS MONGODB MEDIANTE UPDATEMANY()

actualizar multiples documentos con un solo método: updateMany()

este metodo carece dee aislamiento: es decir, las actualizaciones seran visibles en cuanto se realicen. por ello por ejemplo no es util en transacciones financieras o similares.

```javascript
// ejemplo de uso con el $set y una fecha
db.birds.updateMany(
    {
        common_name: {
            $in: ["Blue Jay", "Grackle"],
        },
    },
    {
        $set: {
            last_seen: ISODate("2022-01-01"),
        },
    }
);
```

## LECCIÓN 5: ELIMINAR DOCUMENTOS EN MONGODB

puedes eliminar un documento con deleteOne(<filter>) o varios con deleteMany(<filter>)
