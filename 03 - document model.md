# 03 MongoDB and the Document Model

Atlas es como una plataforma en linea (plataforma de datos para desarrolladores multinube) para ver los datos, funciona como un explorer y para hacer configuraciones de las db de mongo db. su nucleo es mongodb database

Documento: unidad basica de almacenamiento de informacion

-   los documentos se muestran en json. sin embargo, al almacenarse, estos se transforman en Bson los cuales aceptan mas tipos de datos como fechas, etc.

Colección: conjunto de documentos  
Base de Datos: conjunto de colecciones

Cada documento dentro de una colección puede tener propios campos o tipos de datos distintos (polymorphic data), que no tengan nada que ver con los documentos que están dentro de dicha colección. es decir, tiene un schema flexible. sin embargo, siempre podemos añadir un constraint de validacion de esquema.

El cluster es como un conjunto de servidores localizados globalmente que soportan a tu db.

Un cluster (refiriendome a aquel que creas dentro de tu proyecto en atlas) puede contener varias databases... y cada databases puede contener colecciones.

Siempre se pondrá un id, aunque no lo especifiques... y en mongodb este es de tipo ObjectId
