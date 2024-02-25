## LECCIÓN 1: INTRODUCCIÓN A LAS TRANSACCIONES ACID

ACID significa:

-   Atomicidad: Garantiza que cada transacción sea "todo o nada" al comprometer datos en una base de datos. Por ejemplo, no queremos que se retire dinero de una cuenta pero no se agregue correctamente a otra.
-   Consistencia: Garantiza que los datos escritos en la base de datos sean coherentes con las restricciones de la base de datos. Por ejemplo, si el saldo de una cuenta no puede ser menor que 0, una transacción fallaría antes de violar esta restricción.
-   Aislamiento: Garantiza que cada transacción que se ejecute concurrentemente deja la base de datos en el mismo estado que si las transacciones se ejecutaran secuencialmente. En otras palabras, múltiples transacciones pueden ocurrir al mismo tiempo sin afectar el resultado de las otras transacciones.
-   Durabilidad: Garantiza que los datos nunca se pierdan. Los datos se guardan en memoria no volátil, por lo que cualquier modificación realizada a los datos por una transacción exitosa persistirá, incluso en caso de un fallo de energía o hardware.

las transacciones acid son: Un grupo de operaciones de base de datos que deben realizarse todas juntas o no realizarse en absoluto.

## LECCIÓN 2: TRANSACCIONES ACID EN MONGODB

las siguientes afirmaciones son ciertas sobre las transacciones de múltiples documentos en MongoDB

-   Las operaciones de base de datos que afectan a más de un documento, como .updateMany(), no son inherentemente atómicas en MongoDB y deben completarse mediante una transacción de múltiples documentos para tener propiedades ACID.
-   Las transacciones de varios documentos deben tratarse como una herramienta precisa que se utiliza sólo en determinados escenarios.
-   El uso de una transacción de múltiples documentos con una base de datos MongoDB garantiza que la base de datos estará en un estado consistente después de ejecutar un conjunto de operaciones en múltiples documentos.

en una transaccion bancaria por ejemplo se necesita usar una transacción porque las operaciones de múltiples documentos NO son inherentemente atómicas en MongoDB.

## LECCIÓN 3: USO DE TRANSACCIONES EN MONGODB

https://learn.mongodb.com/learn/course/mongodb-transactions/lesson-3-using-transactions-in-mongodb/learn?client=customer&page=2