## LECCIÓN 1: INSERTAR DOCUMENTOS EN UNA COLECCIÓN MONGODB

hay 2 formas de insertar documentos **en 1 sola coleccion**:

-   insertOne(): inserta documentos (datos) en una sola colección

    -   lo agregas asi: db.<collection>.insertOne()
    -   si tu coleccion no existe, entonces se crea
    -   todo documento de una coleccion debe tener el campo \_id y debe ser unique. si no proporciona un id entonces mongo db proporcionara uno por ti. en este caso al agregar documentos, mongo db crea un object id y lo asigna a este documento

-   insertMany(): insertando mas de un documento (datos) en una coleccion.
    -   lo agregas asi: db.<collection>.insertMany({
        <document 1>, <document 2>, <document 3>
        })

## LECCIÓN 2: ENCONTRAR DOCUMENTOS EN UNA COLECCIÓN MONGODB

para encontrar documentos:

-   db.<collection>.find(): para encontrar todos los documentos
-   db.<collection>.find({field: <value>}): para todos los documentos con el valor del campo específico. por ejemplo db.zips.find({state: "AZ})
-   db.<collection>.find({field: {$in: [<value>, <value>, ...]}}): para encontrar todos los documentos con los valores del capo específico

recuerda que por defecto el $eq está implicito si no lo pones, porque si lo pusieras tendrías algo asi: db.<collection>.find({field: $eq: <value>})

## LECCIÓN 3: ENCONTRAR DOCUMENTOS UTILIZANDO OPERADORES DE COMPARACIÓN

Tenemos los siguientes operadores de comparación:

-   $gt (greater than)
-   $gte (greater than or equal to)
-   $lt (less than)
-   $lte (less than or equal to)

Estos se usan de la siguiente manera: `<field>: { <operator>: <value> }`

Recuerda que en el campo "field" puedes acceder a subcampos con el punto: field.subfield

## LECCIÓN 4: CONSULTA DE ELEMENTOS DE ARRAY EN MONGODB

Realizando tu consulta para que solo te bote aquellas que coindicen y que ademas sean un array: Usando el $elemMatch. aqui le decimos que field es un array

-   db.<collection>.find({field: {$elemMatch: { $eq: <value>}}}): asi nos traemos el documento que en cuyo campo, de productos por ejemplo, EL CUAL ES UN ARRAY (indicado por $elemMatch)... que dentro de sus valores tiene el elemento <value>

también puedes ponerle varios query dentro del campo field para que cumplan dichas condicionespor ejemplo:

```js
db.<collection>.find({field: {$elemMatch: { <field>: <value>, <field>: <value>, ...}}})

// ejemplo de uso
db.account.find({products: {$elemMatch: { name: "laptop", price: { $gt: 800}, ... }}})

// ejemplo correcto de uso
db.accounts.find({"products": {$elemMatch: {$eq: "CurrencyService"}}})
```

## LECCIÓN 5: ENCONTRAR DOCUMENTOS UTILIZANDO OPERADORES LÓGICOS

operadores logicos:

-   $and
-   $or

ejemplo:

```js
// versión larga
db.<collection>.find({
    $and: [
        {<expression>},
        {<expression>},
        ...
    ]
})

// versión simplificada (o explicita)
db.collection.find({<expression>, <expression>})

// ejemplo
db.routes.find({
    $and: [
        { $or: [
            {field: <value>},
            {field: <value>}
        ]},
        { $or: [
            {field: <value>},
            {field: <value>}
        ]},
    ]
})
```
