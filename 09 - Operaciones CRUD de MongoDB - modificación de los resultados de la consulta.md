## LECCIÓN 1: ORDENAR Y LIMITAR LOS RESULTADOS DE CONSULTAS EN MONGODB

un cursor apunta a un conjunto de resultados de una consulta. por ejemplo cuando hacemos .find()  
estos metodos como cursor.sort se pueden encadernar a find o findone, etc

al hacer sort establecemos el campo a sort ear... **y el 1 significa ascendente, el -1 descendente**

añadiendo una proyeccion hacemos que sea mas facil de leer.

existe cursor.sort() y cursor.limit()

ejemplos

```javascript
// Return the three music companies with the highest number of employees. Ensure consistent sort order.
// el _id supuestamente es para hacer mas coherente el sort
db.companies
    .find({ category_code: "music" })
    .sort({ number_of_employees: -1, _id: 1 })
    .limit(3);

// ejemplo de uso del sort y limit
db.sales
    .find({
        "items.name": { $in: ["laptop", "backpack", "printer paper"] },
        storeLocation: "London",
    })
    .sort({ saleDate: -1 })
    .limit(3);
```

## LECCIÓN 2: DEVOLVER DATOS ESPECÍFICOS DE UNA CONSULTA EN MONGODB

devolver datos o campos especificos en vez de todos en una consulta... se le llama proyeccion.

**1 para incluir el campo, 0 para excluirlo**

la inclusion y exclusion no pueden combinarse en las proyecciones. excepto el campo \_id

ejemplos

```javascript
// include
db.collection.find(<query>, {<field>: 1, <field>: 1});

// exclude
db.collection.find(<query>, {<field>: 0, <field>: 0});

// ejemplo

db.sales.find({ "customer.age": { $lt: 30 }, "customer.satisfaction": { $gt: 3 }, }, { "customer.satisfaction": 1, "customer.age": 1, "storeLocation": 1, "saleDate": 1, "_id": 0, });

// ejemplo 2
db.sales.find({ storeLocation: { $in: ["Seattle", "New York"] }, }, { couponUsed: 0, purchaseMethod: 0, customer: 0, })
```

## LECCIÓN 3: CONTAR DOCUMENTOS EN UNA COLECCIÓN MONGODB

db.collection.countDocuments(<query>, <options>)

```javascript
// ejemplo
// Count number of trips over 120 minutes by subscribers
db.trips.countDocuments({ tripduration: { $gt: 120 }, usertype: "Subscriber" });

// asi se usa si items pej es un array.. Find the number of sales that included a laptop that cost less that $600.(Forgot the command? Check the hint below!)
db.sales.countDocuments({
    items: { $elemMatch: { name: "laptop", price: { $lt: 600 } } },
});
```
