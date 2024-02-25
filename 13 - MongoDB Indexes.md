## LECCIÓN 1: USO DE ÍNDICES MONGODB EN COLECCIONES

los indices son estructuras de datos especias que almacenan una pequeña porcion de datos, que es facil de recorrer y buscar eficientemente.  
se usan para mejorar el rendimientos de los querys, por ejemplo acelerarlos, reducir E/S de disco.

sin indices: mongodb lee todos los documentos (escaneo de colecciones), y ordena los resultados en memoria.  
con indices: mongodb solo recupera los documentos identificados segun el indice basandose en el query... entregando los resultados mas rapido.

por defecto solo hay 1 indice creado por defecto para cada coleccion, este es el \_id.

los indices impactan en el rendimiento de escritura, estos deben ser actualizdaos cuando se modifica o agregan datos a mi db.  
también asegurate de solo tener los indices que estas utilizando.  
Los índices mejoran el rendimiento de las consultas a costa del rendimiento de la escritura.

tipos de indices:

-   de un solo campo: incluyen solo un campo
-   compuesto: incluyen mas de un campo

ambos tipos de indices también pueden ser indices de varias claves si operan en un campo de matriz... los indices que operan en un campo de matriz se denominan indices multiclave.

## LECCIÓN 2: CREAR UN ÍNDICE DE CAMPO ÚNICO EN MONGODB

desde atlas, en las colecciones, también puedes ver los indices que tienes en tu coleccion.

con explain puedes ver si una consulta esta utilizando los indices... a traves del winningPlan, en inputStage, saldra stage, keypattern e indexname.

```javascript
// crear un index de un solo campo
db.customers.createIndex({
    birthdate: 1,
});
// crear un index de un solo campo pero unico
db.customers.createIndex({ email: 1 }, { unique: true });

// ver los indices usados en una coleccion
db.customers.getIndexes();
// checa si un indice esta siendo usado en un query
// Use explain() in a collection when running a query to see the Execution plan. This plan provides the details of the execution stages (IXSCAN , COLLSCAN, FETCH, SORT, etc.).
// Un winningPlan es un documento que contiene información sobre la consulta y el método que se utilizó para ejecutar la consulta.

db.customers.explain().find({
    birthdate: {
        $gt: ISODate("1995-08-01"),
    },
});
/*
- La IXSCAN etapa indica que la consulta está utilizando un índice y qué índice se está seleccionando.
- La COLLSCAN etapa indica que se realiza un análisis de la colección, sin utilizar ningún índice.
- El FETCHescenario indica que se están leyendo documentos de la colección.
- La SORTetapa indica que los documentos se están clasificando en la memoria.
*/
```

## LECCIÓN 3: CREAR UN ÍNDICE MULTICLAVE EN MONGODB

Un índice multiclave es un indice donde uno de los campos indexados contiene una matriz.

hay una limitacion de solo un campo de matriz por indice... por lo que si un indice tiene multiples campos, solo uno de ellos puede ser una matriz.

```javascript
// creando un indice multiclave de un solo campo.
// en este caso, accounts es un array

db.customers.createIndex({
    accounts: 1,
});
```

Los índices multiclave admiten consultas eficientes en campos de matriz mediante la creación de una clave de índice para cada elemento de la matriz. Esto permite a MongoDB buscar la clave de índice de cada elemento en la matriz en lugar de escanear toda la matriz, lo que resulta en mejoras dramáticas en el rendimiento de sus consultas.

El número máximo de campos de array por índice multiclave es 1. Si un índice tiene varios campos, solo uno de ellos puede ser una matriz.

## LECCIÓN 4: TRABAJAR CON ÍNDICES COMPUESTOS EN MONGODB

el orden de los campos en un indice compuesto importa. sigue este orden: igualdad, orden, rango. ADEMAS, si requerimos resultados ordenados específicos en nuestras consultas, el orden de clasificacion de los valores de campo en el index también importan para evitar las clasificaciones en memoria.

-   equality: prueban coincidencias exactas en un unico valor. este siempre debe colocarse primero en un indice compuesto. ventajas: reduce el tiempo de procesamiento y recupera menos documentos.
-   sort: determinan el orden de los resultados. estos campos deben incluirse despues del campo de equality. el orden de clasificacion de estos valores de campo en el indice es importante si los resultados de la consulta se clasifican por mas de un campo y mezclan ordenes de clasificacion.

###

Un índice compuesto es un índice que contiene referencias a múltiples campos dentro de un documento. Los índices compuestos se crean agregando una lista de campos separados por comas y su orden de clasificación correspondiente a la definición del índice. o sea si algunos campos estan en orden ascendente y otros estan en orden descendente.

El orden recomendado de los campos indexados en un índice compuesto es Igualdad, Ordenación y Rango. Las consultas optimizadas utilizan el primer campo del índice, Igualdad, para determinar qué documentos coinciden con la consulta. El segundo campo del índice, Ordenar, se utiliza para determinar el orden de los documentos. El tercer campo, Rango, se utiliza para determinar qué documentos incluir en el conjunto de resultados.

```javascript
// create a compound index using the `account_holder`, `balance` and `account_type` fields:
db.accounts.createIndex({ account_holder: 1, balance: 1, account_type: 1 });

// Use the explain method to view the winning plan for a query
db.accounts
    .explain()
    .find(
        { account_holder: "Andrea", balance: { $gt: 5 } },
        { account_holder: 1, balance: 1, account_type: 1, _id: 0 }
    )
    .sort({ balance: 1 });
```

## LECCIÓN 5: ELIMINAR ÍNDICES DE MONGODB

```javascript
// borrar un indice
db.customers.dropIndex("active_1_birthdate_-1_name_1");

// borrar todos los indices
db.customers.dropIndexes();

// borrar una lista de indices
db.collection.dropIndexes(["index1name", "index2name", "index3name"]);
// hide index
// El comando hideIndex() oculta un índice. Al ocultar un índice, podrá evaluar el impacto de eliminar el índice en el rendimiento de la consulta. MongoDB no utiliza índices ocultos en las consultas pero continúa actualizando sus claves. Esto le permite evaluar si la eliminación del índice afecta el rendimiento de la consulta y mostrar el índice si es necesario. Mostrar un índice es más rápido que recrearlo. En este ejemplo, usaría el comando db.customers.hideIndex({email:1}).
```
