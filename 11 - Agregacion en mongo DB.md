## LECCIÓN 1: INTRODUCCIÓN A LA AGREGACIÓN DE MONGODB

la agregacion es un analisis y resumen de los datos en mongo db.
agregattion pipeline: es una serie de etapas de agregacion que se completan una despues de otra, en orden

```javascript
// estructura de un pipeline aggregation
db.collection.aggregate([
    {
        $stage1: {
            { expression1 },
            { expression2 }...
        },
        $stage2: {
            { expression1 }...
        }
    }
])

```

etapas de aggregation: cada etapa es una unica operacion de datos. puede contener por ejemplo: math, sort, group

## LECCIÓN 2: USO DE LAS ETAPAS $MATCH Y $GROUP EN UNA CANALIZACIÓN DE AGREGACIÓN DE MONGODB

match y group son etapas.
match filtra documentos con condiciones específicas.
group agrupa documentos por una clave de grupo. (o seea los aggrupa, evitando duplicados)

```javascript
// ejemplo de estructura
db.zips.aggregate([
    {
        $match: {
            state: "CA",
        },
    },
    {
        $group: {
            _id: "$city",
            totalZips: { $count: {} },
        },
    },
]);

// buen ejemplo de uso
db.sightings.aggregate([
    {
        $match: {
            species_common: "Eastern Bluebird",
        },
    },
    {
        $group: {
            _id: "$location.coordinates",
            number_of_sightings: {
                $count: {},
            },
        },
    },
])[
    // el anterior codigo da como resultado esto:
    ({ _id: [40, -74], number_of_sightings: 3 },
    { _id: [41, -74], number_of_sightings: 1 },
    { _id: [40, -73], number_of_sightings: 1 })
];
```

## LECCIÓN 3: USO DE LAS ETAPAS $SORT Y $LIMIT EN UNA CANALIZACIÓN DE AGREGACIÓN DE MONGODB

sort se puede usar con 1 y -1. limit con numero... lo cotidiano

```javascript
// ejemplo de uso de sort y limit
db.zips.aggregate([
    {
        $sort: {
            pop: -1,
        },
    },
    {
        $limit: 5,
    },
]);

// ejemplo de uso practico
db.sightings.aggregate([
    {
        $sort: {
            "location.coordinates.1": -1,
        },
    },
    {
        $limit: 4,
    },
]);
```

## LECCIÓN 4: USO DE LAS ETAPAS $PROJECT, $COUNT Y $SET EN UN CANAL DE AGREGACIÓN MONGODB

project: determina la forma del documento de salida
set: actualiza
count: cuenta

$set se utiliza para crear o cambiar valores de campos nuevos o existentes. $project se puede usar para crear o cambiar el valor de los campos, pero también se puede usar para especificar qué campos mostrar en los documentos en el proceso de agregación.

```javascript
// ! RECUERDA SIEMPRE QUE PARA EL AGGREGATE VA CORCHETES

// project
{
    $project: {
        state:1,
        zip:1,
        population:"$pop",
        _id:0
    }
}
// project ejemplo
db.sightings.aggregate([
  {
    $project: {
        _id: 0,
        species_common: 1,
        date: 1
    }
  }
])

// set
{
    $set: {
        place: {
            $concat:["$city",",","$state"]
        },
        pop:10000
     }
  }
// set ejemplo
db.birds.aggregate([
  {
    $set: {
      'class': 'bird'
    }
  }
])

// count
db.usuarios.aggregate([
    {
        $match: { edad: { $gte: 18 } }
    },
    {
        $count: "totalJOJO"
    }
])


```

```javascript
// ejercicio super completo: consulta el md de dudas count date aggregation
db.sightings.aggregate([
    {
        $match: {
            date: {
                $gt: ISODate("2022-01-01T00:00:00.000Z"),
                $lt: ISODate("2023-01-01T00:00:00.000Z"),
            },
            species_common: "Eastern Bluebird",
        },
    },
    {
        $count: "bluebird_sightings_2022",
    },
]);
```

## LECCIÓN 5: USO DE LA ETAPA $OUT EN UNA CANALIZACIÓN DE AGREGACIÓN DE MONGODB

$out graba lo devuelto en el stage ejecuta... en una nueva coleccion, siempre creandola o sobreescribiendola

```javascript
// usando la otra db
$out: {
    db: "<db>",
    coll: "<newcollection>"
}

// usando la misma db donde estoy
{$out: "<newcollection>"}

// en un aggregate el out va al ultimo

// ejemplo practico de uso
db.sightings.aggregate([
  {
    $match: {
      date: {
        $gte: ISODate('2022-01-01T00:00:00.0Z'),
        $lt: ISODate('2023-01-01T00:00:00.0Z')
      }
    }
  },
  {
    $out: 'sightings_2022'
  }
])
db.sightings_2022.findOne()
```
