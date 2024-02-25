## LECCIÓN 1: USO DE CADENAS DE CONEXIÓN MONGODB

cadena de conexion: nos permite conectarnos a nuestro cluster y trabajar con nuestros datos.
podemos conectarnos desde cualquier medio, shell de mongo, atlas sql, etc.

2 formatos para la cadena de conexion:

-   formato estandar: conectarnos a clusters.
-   dns seed list format: proporciona una conexion dns, y mas ventajas de flexibilidad.

la cadena de conexion la encuentras al querer conectar tu base de datos (en atlas), y al darle a la opcion de "driver".

## LECCIÓN 2: CONECTARSE A UN CLUSTER ATLAS DE MONGODB CON EL SHELL

El entorno REPL que utiliza MongoDB Shell es node

## LECCIÓN 3: CONECTARSE A UN CLUSTER ATLAS DE MONGODB CON COMPASS

COMPASS ES UN GUI digamos visual que nos permite realizar tareas de consulta, etc. es lo que usé en un proyecto de un curso de udemy

mongo db compass nos permite:

-   consultar datos, eliminarlos, editarlos, agregarlos, etc.
-   analizar datos, etc.
-   nos permite conectarnos a nuestro cluster y ver las db y colecciones que hay dentro, asi como los documentos.

## LECCIÓN 4: CONECTARSE A UN CLUSTER ATLAS DE MONGODB DESDE UNA APLICACIÓN

la conexion mediante "drivers" permite conectar nuestra aplicacion a la db mediante cualquier lenguaje de programacion

para saber como conectarte, puedes leer la documentacion o en mongodb developer center. también puedes aprender a hacerlo mediante mongodb university.

## LECCIÓN 5: SOLUCIÓN DE PROBLEMAS DE ERRORES DE CONEXIÓN DE MONGODB ATLAS

podemos presentar problemas por ejemplo porque nuestro ip addres no esta permitido para conectarse a nuestra db, porque colocamos mal la contraseña
