### RESUMEN DETALLADO DE LOS CAPÍTULOS 3, 4, 5 Y 6 DEL LIBRO "The Web of Data"

---

## Capítulo 3: RDF - Resource Description Framework

### Propósito

RDF (Marco de Descripción de Recursos) fue diseñado por el W3C como un modelo para representar información estructurada sobre recursos en la Web. A diferencia del HTML, que está pensado para que los humanos lean documentos, RDF está diseñado para que las máquinas entiendan datos.

Es un paso fundamental hacia una Web donde los agentes automáticos pueden encontrar, combinar y reutilizar información de forma inteligente, sin necesidad de interpretación manual.

### Modelo de Datos

El modelo RDF se basa en grafos. La información se representa como conjuntos de "tripletas", una estructura sencilla pero poderosa que describe relaciones entre entidades:

- **Sujeto**: el recurso del cual se habla (identificado por una URI o blank node).
    
- **Predicado**: la propiedad o relación que se está describiendo (siempre es una URI).
    
- **Objeto**: el valor de esa propiedad (puede ser otra URI, un literal, o un blank node).
    

**Ejemplo simple:**

```
<http://ex.org/Ana> <http://xmlns.com/foaf/0.1/name> "Ana"
```

Este enunciado indica que el recurso identificado como "Ana" tiene como nombre "Ana". Las URIs aseguran una identificación única global.

**Analogía:** Es como una oración simple con sujeto, verbo y objeto: "Ana tiene nombre Ana".

### Tipos de Términos RDF

1. **IRIs (Internationalized Resource Identifiers)**: similares a URLs, se usan para identificar de forma única los recursos.
    
2. **Literals**: valores concretos como cadenas de texto, números, fechas. Se pueden tipar con XSD (XML Schema Datatypes).
    
3. **Blank Nodes**: nodos anónimos usados cuando un recurso no necesita o no tiene identificador global. Ejemplo: una dirección postal.
    

### Grafos RDF

Un conjunto de tripletas forma un grafo etiquetado, dirigido. Cada nodo es un recurso, y cada arista representa una propiedad. Esto hace que RDF sea ideal para integrar información desde diversas fuentes.

### Serializaciones RDF

- **RDF/XML**: la primera recomendación oficial, basada en XML. Verboso y poco intuitivo.
    
- **Turtle**: formato compacto, fácil de leer para humanos.
    
- **N-Triples**: una tripleta por línea, útil para procesamientos automáticos.
    
- **JSON-LD**: integra RDF con aplicaciones web modernas basadas en JSON.
    

**Ejemplo en Turtle:**

```turtle
@prefix foaf: <http://xmlns.com/foaf/0.1/> .
<http://ex.org/Ana> foaf:name "Ana" .
```

### Contenedores y Colecciones

RDF permite modelar grupos de elementos:

- **rdf:Bag**: conjunto sin orden.
    
- **rdf:Seq**: secuencia ordenada (como una lista).
    
- **rdf:Alt**: opciones alternativas.
    
- **rdf:List**: listas enlazadas con una estructura recursiva basada en `rdf:first` y `rdf:rest`.
    

**Ejemplo:** una lista de ingredientes en una receta.

### Vocabularios RDF

- **FOAF**: describe personas, sus relaciones y redes sociales.
    
- **Dublin Core**: conjunto de metadatos usados para describir recursos como documentos, imágenes, etc.
    

---

## Capítulo 4: RDF Schema (RDFS) y Semántica

### Propósito

RDFS proporciona una semántica básica a los datos RDF mediante un conjunto de clases y propiedades predefinidas. Define estructuras tipo "ontologías ligeras" y permite construir vocabularios comunes para describir tipos de datos y relaciones.

### Elementos Principales

- **rdfs:Class**: define una categoría (ej. "Persona").
    
- **rdf:type**: indica que un recurso es instancia de una clase.
    
- **rdfs:subClassOf**: define jerarquías entre clases. Si A es subclase de B, todo A también es B.
    
- **rdfs:Property**: define una propiedad.
    
- **rdfs:subPropertyOf**: permite propiedades jerárquicas.
    
- **rdfs:domain y rdfs:range**: indican los tipos esperados de los sujetos y objetos de una propiedad.
    

**Ejemplo:**

```turtle
ex:esPadreDe rdfs:domain ex:Persona ;
               rdfs:range ex:Persona .
```

Indica que "esPadreDe" es una relación entre dos personas.

### Inferencia Automática

Los razonadores pueden deducir hechos nuevos:

- Si `ex:Ana rdf:type ex:Mujer` y `ex:Mujer rdfs:subClassOf ex:Persona`, entonces `ex:Ana rdf:type ex:Persona`.
    
- Si una propiedad tiene un dominio definido y se usa sobre un recurso, se puede inferir que ese recurso es de dicha clase.
    

### Analogía

Es como la herencia en programación orientada a objetos. Una clase hija hereda los atributos de la clase padre.

### Limitaciones

RDFS no puede expresar:

- Cardinalidades (mínimo/máximo de valores).
    
- Relación entre propiedades.
    
- Negaciones, disyunciones.  
    Para estos casos, se usa OWL.
    

---

## Capítulo 5: OWL - Web Ontology Language

### Propósito

OWL es un lenguaje basado en lógica descriptiva que permite construir ontologías complejas y ricas en semántica. Se utiliza para describir conceptos, relaciones, restricciones y axiomas sobre un dominio.

### Versiones de OWL

- **OWL Full**: sin restricciones, pero razonamiento indecidible.
    
- **OWL DL**: balance entre expresividad y decidibilidad. Basado en lógica descriptiva.
    
- **Perfiles OWL 2**: variantes especializadas para diferentes necesidades computacionales:
    
    - **OWL EL**: eficiente para jerarquías extensas.
        
    - **OWL QL**: optimizado para consultas.
        
    - **OWL RL**: para reglas e implementaciones mediante motores de reglas.
        

### Componentes

- **Clases**: conceptos del dominio. Permite intersección (`owl:intersectionOf`), unión (`owl:unionOf`), complemento (`owl:complementOf`), etc.
    
- **Propiedades**: de objeto y de datos. Pueden ser transitivas, simétricas, funcionales, inversas, etc.
    
- **Restricciones**:
    
    - **owl:someValuesFrom**: debe existir al menos un valor.
        
    - **owl:allValuesFrom**: todos los valores deben pertenecer a una clase.
        
    - **owl:cardinality**: cantidad exacta de valores.
        

**Ejemplo:** Una persona tiene al menos un nombre:

```turtle
ex:Persona rdfs:subClassOf [
  a owl:Restriction ;
  owl:onProperty ex:nombre ;
  owl:minCardinality 1
] .
```

### Razonamiento

- Detecta errores lógicos: un individuo que pertenece a dos clases disjuntas.
    
- Descubre relaciones implícitas.
    

**Analogía:** OWL actúa como una "mente lógica" que verifica que lo que decimos tiene sentido y deduce lo que se infiere lógicamente.

---

## Capítulo 6: SPARQL - Lenguaje de Consulta

### Propósito

SPARQL (SPARQL Protocol and RDF Query Language) es el lenguaje oficial para consultar y modificar grafos RDF. Permite hacer preguntas complejas sobre conjuntos de datos distribuidos.

### Consulta Básica

```sparql
SELECT ?nombre WHERE {
  ?persona rdf:type foaf:Person .
  ?persona foaf:name ?nombre .
}
```

Busca los nombres de todos los recursos que son personas.

### Cláusulas comunes

- **FILTER**: condiciones lógicas, comparaciones.
    
- **OPTIONAL**: permite que datos faltantes no invaliden la consulta.
    
- **UNION**: combinación de patrones.
    
- **ORDER BY**, **LIMIT**, **OFFSET**: control del formato de salida.
    

### Funciones Avanzadas

- **Subconsultas**: consultas anidadas.
    
- **Agregación**: `COUNT`, `AVG`, `GROUP BY`, `HAVING`.
    
- **Negación**: `NOT EXISTS`, `MINUS`.
    
- **Property Paths**: consultas sobre relaciones transitivas o repetidas.
    

### Actualizaciones

- **INSERT DATA**: agregar triples.
    
- **DELETE DATA**: eliminar triples.
    
- **MODIFY**: combinar inserciones y eliminaciones.
    

**Ejemplo:**

```sparql
INSERT DATA {
  <http://ex.org/Ana> foaf:age "30"^^xsd:integer
}
```

### Federación

Permite consultar varios endpoints SPARQL al mismo tiempo:

```sparql
SERVICE <http://dbpedia.org/sparql> {
  ?x dbo:birthPlace dbr:Chile .
}
```

**Analogía:** SPARQL es a RDF lo que SQL es a las bases relacionales, pero adaptado a la flexibilidad y distribución de la Web.

---

Este resumen con explicaciones ampliadas, ejemplos y analogías proporciona una base completa para el estudio de las tecnologías centrales de la Web de Datos.