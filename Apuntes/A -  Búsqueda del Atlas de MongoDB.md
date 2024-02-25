## LECCIÓN 1: USO DE BÚSQUEDAS BASADAS EN RELEVANCIA E ÍNDICES DE BÚSQUEDA

Un índice de búsqueda se utiliza para describir cómo debería funcionar el algoritmo de búsqueda de la aplicación. Puedes personalizar esto con Atlas Search.

Los indices de busqueda se utilizan para definir como se debe realizar una busqueda basada en la relevancia.

## LECCIÓN 2: CREAR UN ÍNDICE DE BÚSQUEDA CON MAPEO DINÁMICO

puedes crear un indice de bisqueda a traves de las colecciones del atlas. esto te servira para realizar una mejor busqueda en la barra search.

puedes escoger mapeo dinamico o estatico

Las asignaciones dinámicas especifican que Atlas Search debe indexar automáticamente los campos indexables de una colección.

```javascript
Explanation:

The /app/search_index.json file contains the Atlas search index definition. Here is an overview of the fields defined:

The name field specifies the name of the index as it will appear in MongoDB Atlas.
The searchAnalyzer specifies the analyzer to apply to query text before searching with it. In this example, we use the default lucene.standard analyzer.
The analyzer defines how Atlas search will turn a string field's contents into searchable terms. It defines the tokenizer used to extract tokens from the > text and filters to use. In this example, we use the default lucene.standard analyzer.
The collectionName and database define the collection and database on which to create the index.
The mappings object is used to define the mapping to use, dynamic or static. In this example, we are using dynamic which means that we want Atlas > Search to automatically index all supported field types.
```

**Una búsqueda con un índice dinámico consultará todos los campos, incluidos los campos anidados.**

utiliza un índice de búsqueda asignado dinámicamente CUANDO desee buscar en todos los campos con el mismo peso...... El mapeo de campos dinámico se utiliza para buscar en todos los campos el término de búsqueda, con el mismo peso asignado a todos los campos.

## LECCIÓN 3: CREAR UN ÍNDICE DE BÚSQUEDA CON ASIGNACIÓN DE CAMPOS ESTÁTICOS

Si el índice de búsqueda está asignado estáticamente y el único campo asignado es el campo "storeLocation", y usted buscó uno de los artículos vendidos por la empresa de suministros de oficina, los blocs de notas... NO APARECERÁ NINGUN RESULTADO... porque El único campo indexado es la ubicación de la tienda, por lo que datos como los artículos vendidos no han sido indexados por el algoritmo de búsqueda. Solo se devolverán consultas de nombres de ciudades presentes como valores en el campo storeLocation.

```javascript
// aqui se usa un mapeo estatico. Como puede ver, "fields" hay configuraciones específicas para el "common_name" campo. Los índices de búsqueda con mapeo de campos estáticos están diseñados para encontrar resultados de campos específicos.

{
    "mappings": {
        "dynamic": false,
        "fields": {
            "common_name": [
            {
                "dynamic": true,
                "type": "document"
            },
            {
                "type": "string"
            }
            ]
        }
    }
}
```

## LECCIÓN 4: USO DE $SEARCH Y OPERADORES COMPUESTOS

"debe" excluirá los registros que no cumplan con los criterios. "mustNot" excluirá los resultados que cumplan con los criterios. "should" le permitirá dar peso a los resultados que cumplan con los criterios para que aparezcan primero.La cláusula "filter" devuelve resultados que coinciden con la cláusula. Elimina resultados que no coinciden con la cláusula.

cláusulas utilizadas por el operador compuesto contribuyen a la puntuación dada los resultados: "must", "must not", and "should"... "filtrar" no afecta la puntuación otorgada a los resultados.

## LESSON 5: GROUP SEARCH RESULTS BY USING FACETS

$searchMeta es una etapa de agregación para Atlas Search donde se muestran los metadatos relacionados con la búsqueda. Esto significa que si nuestros resultados de búsqueda se dividen en depósitos, usando $facet, podemos verlo en la etapa $searchMeta, porque esos depósitos son información sobre cómo están formateados los resultados de la búsqueda.

Si deseo ver los metadatos (facetas y su recuento) para Atlas Search, debo usar la etapa de agregación llamada: $searchMeta

¿Qué operador puedes utilizar para agrupar los resultados de búsqueda de Atlas? (Seleccione uno.)  
facet
