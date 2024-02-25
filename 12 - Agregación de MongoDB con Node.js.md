## LECCIÓN 1: CREACIÓN DE UN CANAL DE AGREGACIÓN DE MONGODB EN APLICACIONES NODE.JS

canal de agregacion o aggregation pipelin ese usa para crear query multi etapas.

si la consulta o query puede ejecutarse con un simple find()... entonces usalo. pero si requieres proceso adicional, entonces usa el aggregation, dado que es mas avanzado.

Las operaciones de agregación procesan registros de datos y devuelven resultados calculados. Cuando trabaje con datos en MongoDB, es posible que deba ejecutar rápidamente operaciones complejas que involucran múltiples etapas para recopilar métricas para su proyecto. Generar informes y mostrar metadatos útiles son sólo dos casos de uso importantes en los que las operaciones de agregación de MongoDB pueden ser increíblemente útiles, poderosas y flexibles.

Un canal de agregación consta de una o más etapas que procesan documentos. Cada etapa realiza una operación en los documentos de entrada. Por ejemplo, una etapa puede filtrar documentos, agrupar documentos o calcular valores.

## LESSON 2: USING MONGODB AGGREGATION STAGES WITH NODE.JS: $MATCH AND $GROUP

RECUERDA QUE SIEMPRE EN UN PIPELINE, es un array, y dentro VAN el match y group por separado con su propio "{}"

```javascript
// ejemplo de uso de $match y $group
const pipeline = [
    // Stage 1: match the accounts with a balance lesser than $1,000
    { $match: { balance: { $lt: 1000 } } },
    // Stage 2: Calculate average balance and total balance for each type of account
    {
        $group: {
            _id: "$account_type",
            total_balance: { $sum: "$balance" },
            avg_balance: { $avg: "$balance" },
        },
    },
];
```

## LECCIÓN 3: USO DE ETAPAS DE AGREGACIÓN DE MONGODB CON NODE.JS: $SORT Y $PROJECT

```javascript
const pipeline = [
    // Stage 1: $match - filter the documents (checking, balance >= 1500)
    { $match: { account_type: "checking", balance: { $gte: 1500 } } },

    // Stage 2: $sort - sorts the documents in descending order (balance)
    { $sort: { balance: -1 } },

    // Stage 3: $project - project only the requested fields and one computed field (account_type, account_id, balance, gbp_balance)
    {
        $project: {
            _id: 0,
            account_id: 1,
            account_type: 1,
            balance: 1,
            // GBP stands for Great British Pound
            gbp_balance: { $divide: ["$balance", 1.3] },
        },
    },
];

let result = await accounts.aggregate(pipeline);
```
