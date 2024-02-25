# 04 MongoDB Data Modeling Intro

## Tipos de relaciones de datos

> los datos a los que se acceden juntos se deben almacenar juntos.

**tipos de relaciones**: existen relaciones de uno a muchos, muchos a muchos y uno a uno.

**dos formas principales de modelar las relaciones de datos** en mongo db son embedding (por ejemplo incrustar como tal los nombres mediante un array en un documento) y referencing (por ejemplo referenciar con object id a otro documento).

## Incrustar Datos en documentos

incursitar (embedding) datos en documentos **simplifica los querys o consultas o mejorar el rendimientos de estas**... pero hace que el documento sea mas grande con el tiempo, pudiendo llegar algo ilimitado puediendo llegar al limite de tamaño de un archivo bson

**los tipos de relaciones que se suelen utilizar en el embedding o incrustacion son**: relacion 1-1, 1-muchos y muchos a muchos

**caracteristicas del embedding**:

-   Consulta única para recuperar datos
-   Operación única para actualizar/eliminar datos
-   Duplicación de datos
-   Documentos grandes

## LECCIÓN 5: HACER REFERENCIA A DATOS EN DOCUMENTOS

para referenciar un documento o campos con otros se usan los \_id. le "referenciacion" también suele llamarse "linking" o "data normalization"

**caracteristicas de la referencia**:

-   no hay data duplicada
-   los documentos son mas pequeños
-   las consultas a multiples documentos pueden costar recursos extra o impactar en el rendimiento de la lectura

## LECCIÓN 6: ESCALANDO UN MODELO DE DATOS

recuerda evitar el unbounded documentos (documentos ilimitados) porque estos impactan en el rendimiento y en problemas de almacenamiento.  
la forma de evitarlo es separando la data en multiples colecciones y usar referencias

siempre recuerda el principio de diseño de tu modelo de datos: "la data a la que accedes junta, debe ser almacenada junta".

## LECCIÓN 7: USO DE HERRAMIENTAS ATLAS PARA AYUDA CON ESQUEMAS

existen diseño de esquemas antipatrones a evitar en tu base de datos.

en la pestaña de colecciones (del data explorer), puedes ver las pestaña de indexes asi como también la pestaña de "schema anti-patterns" (para mejorar tus schemas), son ayudas utiles, también se vinculan a documentacion de ayuda.

otra herramienta util es el atlas performance advisor, es una opcion de pago digamos disponible solo para aquellos con nivel M10.
