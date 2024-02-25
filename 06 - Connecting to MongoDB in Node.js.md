## LECCIÓN 1: USO DE BIBLIOTECAS DE CLIENTE MONGODB NODE.JS

las aplicaciones de mongodb node.js deben utilizar el driver oficial de mongo db  
Los controladores MongoDB establecen conexiones seguras a un clúster MongoDB y ejecutan operaciones de bases de datos en nombre de las aplicaciones cliente.

los drivers simplifican la coneccion a una db mongo db

la documentacion oficial del driver esta en el website de mongo db

## LECCIÓN 2: CONECTARSE A UN CLUSTER ATLAS EN APLICACIONES NODE.JS

el driver se instala mediante la instalacion del paquete "npm install mongodb". el conection string lo obtienes del connect en la opcion driver

**una aplicacion debe usar una unica instancia de mongoclient para todas las consultas**... para el ciclo de vida de una aplicacion... ya que crear esta instancia es resource intensice... afectando el rendimiento de la aplicacion.

## LECCIÓN 3: SOLUCIÓN DE PROBLEMAS DE UNA CONEXIÓN MONGODB EN APLICACIONES NODE.JS

recuerda que por defecto el cluster no tiene acceso a ningun lugar externo, ni si quiera tu ip
