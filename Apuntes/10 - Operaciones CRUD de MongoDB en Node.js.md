## LECCIÓN 1: TRABAJAR CON DOCUMENTOS MONGODB EN NODE.JS

Binario JSON = BSON

el driver de mongo db en node js nos permite representar los BSON como objetos en javascript.

¿Cómo se convierte BSON a JSON cuando se utiliza el controlador MongoDB para Node.js? (Seleccione uno.)  
El controlador convierte automáticamente los documentos codificados en BSON, lo que le permite utilizar los datos inmediatamente en su aplicación.  
El controlador convierte automáticamente los documentos codificados en BSON. Esto significa que puede usar los datos inmediatamente en su aplicación como JSON normal y acceder a las propiedades mediante notación de puntos. El controlador maneja la conversión de BSON a JSON por usted.

## LECCIÓN 2: INSERTAR UN DOCUMENTO EN APLICACIONES NODE.JS

```javascript
// nuestra coleccion
const accountsCollection = client.db(dbname).collection(collection_name);

// insertOne
let result = await accountsCollection.insertOne(sampleAccount);

// insertMany
let result = await accountsCollection.insertMany(sampleAccounts);
```

## LECCIÓN 3: CONSULTAR UNA COLECCIÓN MONGODB EN APLICACIONES NODE.JS

```javascript
// filtro
const documentsToFind = { balance: { $gt: 4700 } };

// .find()
let result = accountsCollection.find(documentsToFind);
// también puedes usar el findOne()
// conteo
let docCount = accountsCollection.countDocuments(documentsToFind);
```

## LECCIÓN 4: ACTUALIZACIÓN DE DOCUMENTOS EN APLICACIONES NODE.JS

```javascript
// updateOne
const documentToUpdate = { _id: ObjectId("62d6e04ecab6d8e130497482") };

const update = { $inc: { balance: 100 } };

let result = await accountsCollection.updateOne(documentToUpdate, update);

// updateMany
const documentsToUpdate = { account_type: "checking" };

const update = { $push: { transfers_complete: "TR413308000" } };

let result = await accountsCollection.updateMany(documentsToUpdate, update);
```

## LECCIÓN 5: ELIMINAR DOCUMENTOS EN APLICACIONES NODE.JS

```javascript
const accountsCollection = client.db(dbname).collection(collection_name);

// deleteOne
const documentToDelete = { _id: ObjectId("62d6e04ecab6d8e13049749c") };
let result = await accountsCollection.deleteOne(documentToDelete);

// deleteMany
const documentsToDelete = { balance: { $lt: 500 } };
let result = await accountsCollection.deleteMany(documentsToDelete);
```

## LECCIÓN 6: CREACIÓN DE TRANSACCIONES MONGODB EN APLICACIONES NODE.JS

se refiere a un grupo de operaciones que abarcan varios documentos.  
Si una de las operaciones falla, las operaciones que no fallaron no se confirmarán. Se cancelará toda la transacción y no se confirmará ninguna operación.

las transacciones pueden durar como maximo 60 segundos.

El método withTransaction() inicia la transacción, ejecuta la devolución de llamada y luego confirma o cancela la transacción.

```javascript
// ejemplo largo de una transaction

const main = async () => {
    try {
        const transactionResults = await session.withTransaction(async () => {
            // TODO Step 1: Update the sender balance and pass along the `session` object
            const updateSenderResults = await accounts.updateOne(
                { account_id: account_id_sender },
                { $inc: { balance: -transaction_amount } },
                { session }
            );
            console.log(
                `${updateSenderResults.matchedCount} document(s) matched the filter, updated ${updateSenderResults.modifiedCount} document(s) for the sender account.`
            );

            // Step 2: Update the account receiver balance
            const updateReceiverResults = await accounts.updateOne(
                { account_id: account_id_receiver },
                { $inc: { balance: transaction_amount } },
                { session }
            );
            console.log(
                `${updateReceiverResults.matchedCount} document(s) matched the filter, updated ${updateReceiverResults.modifiedCount} document(s) for the receiver account.`
            );

            // Step 3: Insert the transfer document
            const transfer = {
                transfer_id: "TR21872187",
                amount: 100,
                from_account: account_id_sender,
                to_account: account_id_receiver,
            };

            const insertTransferResults = await transfers.insertOne(transfer, {
                session,
            });
            console.log(
                `Successfully inserted ${insertTransferResults.insertedId} into the transfers collection`
            );

            // Step 4: Update the transfers_complete field for the sender account
            const updateSenderTransferResults = await accounts.updateOne(
                { account_id: account_id_sender },
                { $push: { transfers_complete: transfer.transfer_id } },
                { session }
            );
            console.log(
                `${updateSenderTransferResults.matchedCount} document(s) matched in the transfers collection, updated ${updateSenderTransferResults.modifiedCount} document(s) for the sender account.`
            );
            // Step 5: Update the transfers_complete field for the receiver account
            const updateReceiverTransferResults = await accounts.updateOne(
                { account_id: account_id_receiver },
                { $push: { transfers_complete: transfer.transfer_id } },
                { session }
            );
            console.log(
                `${updateReceiverTransferResults.matchedCount} document(s) matched in the transfers collection, updated ${updateReceiverTransferResults.modifiedCount} document(s) for the receiver account.`
            );
        });

        console.log("Committing transaction ...");
        // If the callback for withTransaction returns successfully without throwing an error, the transaction will be committed
        if (transactionResults) {
            console.log("The reservation was successfully created.");
        } else {
            console.log("The transaction was intentionally aborted.");
        }
    } catch (err) {
        console.error(`Transaction aborted: ${err}`);
        process.exit(1);
    } finally {
        await session.endSession();
        await client.close();
    }
};
```
