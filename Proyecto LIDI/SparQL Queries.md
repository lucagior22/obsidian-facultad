
# Obtener el precio de las casas de Brandsen

```sparql
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX inmo: <http://www.semanticweb.org/luciana/ontologies/2024/8/inmontology#>
PREFIX avis: <https://raw.githubusercontent.com/fdioguardi/pronto/main/ontology/pronto.owl#>
PREFIX io:<http://www.semanticweb.org/luciana/ontologies/2024/8/inmontology#>
PREFIX time: <http://www.w3.org/2006/time#>
PREFIX sioc: <http://rdfs.org/sioc/ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX rec: <https://w3id.org/rec#>
PREFIX gr: <http://purl.org/goodrelations/v1#>
PREFIX owl: <http://www.w3.org/2002/07/owl#>

SELECT ?address ?cityLabel ?value ?currency ?priceType
WHERE {
  # Obtengo las propiedades que son Casa y guardo las features en ?feature
  ?realEstateSite rdf:type io:Casa;
    io:hasFeature ?featureAddress.


  # Obtengo los detalles de las features (son blank nodes)
  ?featureAddress io:hasDetail ?bnFeatureAddressDetail.

  
  # Obtengo los ScraperValue de los detalles (son blank nodes)
  ?bnFeatureAddressDetail io:hasScraperValue ?bnAddressScraperValue.

  
  # Obtengo los datos de dirección y ciudad del ScraperValue
  # La dirección es un literal, la ciudad es otro nodo
  ?bnAddressScraperValue  io:address ?address;
    io:city ?city.

  

  # Para hacer legible la información de la ciudad, busco el label
  # Primero filtro por Brandsen y después me guardo los labels en una variable para el select.
  ?city rdfs:label "Brandsen"^^xsd:string;
    rdfs:label ?cityLabel.

  

  # Para buscar el precio, asocio las casas con sus listings que se guardan en ?listing.
  # Además, obtengo los features asociados (precio generalmente)
  ?listing sioc:about ?realEstateSite;
    io:hasFeature ?featurePriceListing.

  

  # Filtro los features solo de precio (suelen ser todos de precio, pero igual)
  # Obtengo el detalle del feature (es un blank node)
  ?featurePriceListing rdf:type io:Precio;
     io:hasDetail ?bnFeaturePriceListingDetail.

  

  # Obtengo los ScraperValue de los detalles (son blank nodes)
  ?bnFeaturePriceListingDetail io:hasScraperValue ?bnPriceScraperValue.

  
  # Obtengo los datos de moneda, valor y tipo de valor del ScraperValue
  # Todos son literales
  ?bnPriceScraperValue gr:hasCurrency ?currency;
    gr:hasCurrencyValue ?value;
    gr:priceType ?priceType.

  

}

LIMIT 200

# ! En el grafo viejo hay 2 listings correspondientes a propiedades en Brandsen, pero solo una es io:Casa

```

---
# Precio de los terrenos por m2 en Chapadmalal (vieja, ver siguiente)

```sparql
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX inmo: <http://www.semanticweb.org/luciana/ontologies/2024/8/inmontology#>
PREFIX avis: <https://raw.githubusercontent.com/fdioguardi/pronto/main/ontology/pronto.owl#>
PREFIX io: <http://www.semanticweb.org/luciana/ontologies/2024/8/inmontology#>
PREFIX time: <http://www.w3.org/2006/time#>
PREFIX sioc: <http://rdfs.org/sioc/ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX rec: <https://w3id.org/rec#>
PREFIX gr: <http://purl.org/goodrelations/v1#>
PREFIX owl: <http://www.w3.org/2002/07/owl#>

# 3. ¿Cuál es el precio en dólares por metro cuadrado de los terrenos en Chapadmalal?
# - Se buscan terrenos.
# - Precio en dólares.
# - Precio por m2.
# - Ubicación Chapadmalal.
# ! La práctica decía La Plata pero en el .ttl no tenemos data de LP

SELECT ?value ?spaceValueM2 ?valueM2Final ?currency ?address ?cityLabel
WHERE {
  # Obtenemos las propiedades que son Terreno
  ?terreno a io:Terreno;
          io:hasFeature ?featureAddress.
          
  # Obtengo los detalles de las features (son blank nodes)
  ?featureAddress io:hasDetail ?bnFeatureAddressDetail.

  # Obtengo los ScraperValue de los detalles (son blank nodes)
  ?bnFeatureAddressDetail io:hasScraperValue ?bnAddressScraperValue.

  # Obtengo los datos de dirección y ciudad del ScraperValue
  # La dirección es un literal, la ciudad es otro nodo
  ?bnAddressScraperValue  io:address ?address;
                          io:city ?city.

  # Para hacer legible la información de la ciudad, busco el label
  # Primero filtro por Chapadmalal y después me guardo los labels en una variable para el select.
  ?city rdfs:label "Chapadmalal"^^xsd:string;
        rdfs:label ?cityLabel.

  # Buscamos los M2
  # Obtenemos el espacio del terreno
  ?terreno rec:includes ?spaceLand.

  # Buscamos los precios
  # Obtenemos los listings asociados a los terrenos y sus features de precio
  ?listings sioc:about ?terreno;
            io:hasFeature ?featurePriceListing.

  # Obtenemos el blank node del detail de las features
  ?featurePriceListing io:hasDetail ?bnFeaturePriceListingDetail.

  # Obtenemos el blank node del ScraperValue
  ?bnFeaturePriceListingDetail io:hasScraperValue ?bnPriceScraperValue.
  {
    # --------- Región INAMOVIBLE para que funcione el cálculo ---------
    # Filtramos los espacios a que sean land (excluimos building sites)
    ?spaceLand a rec:Site;
               io:hasFeature ?featureTotalSpaceLand.

    # Filtramos las features a que sean superfice
    ?featureTotalSpaceLand a io:Superficie.

    # Obtenemos el blank node del detalle de la feature
    ?featureTotalSpaceLand io:hasDetail ?bnFeatureTotalSpaceLandDetail.

    # Obtenemos valor, moneda y filtramos por tipo de precio base (excluimos fees)
    ?bnPriceScraperValue gr:hasCurrency ?currency;
                         gr:priceType "BASE"^^xsd:string;
                         gr:hasCurrencyValue ?value.

    # --------- Fin de Región INAMOVIBLE para que funcione el cálculo ---------
    # --------- HA ---------
    # Obtenemos el blank node del ScraperValue
    ?bnFeatureTotalSpaceLandDetail io:hasScraperValue ?bnSpaceScraperValueHA.

    # Obtenemos los valores de superficie en HA
    ?bnSpaceScraperValueHA gr:hasUnitOfMeasurement "ha"^^xsd:string;
                           gr:hasValue ?spaceValueHA.
      
    BIND(?spaceValueHA * 10000 AS ?spaceValueM2)
    BIND(?value / ?spaceValueM2 AS ?valueM2Final)
    # --------- HA ---------
  }
  UNION
  {
    # --------- Región INAMOVIBLE para que funcione el cálculo ---------
    # Filtramos los espacios a que sean land (excluimos building sites)
    ?spaceLand a rec:Site;
               io:hasFeature ?featureTotalSpaceLand.

    # Filtramos las features a que sean superfice
    ?featureTotalSpaceLand a io:Superficie.

    # Obtenemos el blank node del detalle de la feature
    ?featureTotalSpaceLand io:hasDetail ?bnFeatureTotalSpaceLandDetail.

    # Obtenemos valor, moneda y filtramos por tipo de precio base (excluimos fees)
    ?bnPriceScraperValue gr:hasCurrency ?currency;
                         gr:priceType "BASE"^^xsd:string;
                         gr:hasCurrencyValue ?value.

    # --------- Fin de Región INAMOVIBLE para que funcione el cálculo ---------
    
    # --------- M2 ---------
    # Obtenemos el blank node del ScraperValue
    ?bnFeatureTotalSpaceLandDetail io:hasScraperValue ?bnSpaceScraperValueM2.

    # Obtenemos los valores de superficie en M2
    ?bnSpaceScraperValueM2 gr:hasUnitOfMeasurement "m²"^^xsd:string;
                           gr:hasValue ?spaceValueM2.

    BIND(?value / ?spaceValueM2 AS ?valueM2Final)

    # --------- M2 ---------
  }
}
LIMIT 100

```

# Precio de los terrenos por m2 en Chapadmalal (optimizada)
Esta versión es mejor porque **elimina la duplicación masiva de código** (unas 15 líneas repetidas en el UNION) y **es más eficiente** al usar un solo patrón de búsqueda con lógica condicional en lugar de dos búsquedas separadas.

```sparql
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX inmo: <http://www.semanticweb.org/luciana/ontologies/2024/8/inmontology#>
PREFIX avis: <https://raw.githubusercontent.com/fdioguardi/pronto/main/ontology/pronto.owl#>
PREFIX io: <http://www.semanticweb.org/luciana/ontologies/2024/8/inmontology#>
PREFIX time: <http://www.w3.org/2006/time#>
PREFIX sioc: <http://rdfs.org/sioc/ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX rec: <https://w3id.org/rec#>
PREFIX gr: <http://purl.org/goodrelations/v1#>
PREFIX owl: <http://www.w3.org/2002/07/owl#>

# 3. ¿Cuál es el precio en dólares por metro cuadrado de los terrenos en Chapadmalal?
# - Se buscan terrenos.
# - Precio en dólares.
# - Precio por m2.
# - Ubicación Chapadmalal.
# ! La práctica decía La Plata pero en el .ttl no tenemos data de LP

SELECT ?value ?spaceValue ?unit ?valueM2 ?currency ?address ?cityLabel
WHERE {
  # --------- FILTRADO POR TIPO DE PROPIEDAD ---------
  # Obtenemos las propiedades que son Terreno
  ?terreno a io:Terreno;
          io:hasFeature ?featureAddress.
          
  # --------- INFORMACIÓN DE UBICACIÓN ---------
  # Obtengo los detalles de las features de dirección (son blank nodes)
  ?featureAddress io:hasDetail ?bnFeatureAddressDetail.

  # Obtengo los ScraperValue de los detalles de dirección (son blank nodes)
  ?bnFeatureAddressDetail io:hasScraperValue ?bnAddressScraperValue.

  # Obtengo los datos de dirección y ciudad del ScraperValue
  # La dirección es un literal, la ciudad es otro nodo
  ?bnAddressScraperValue  io:address ?address;
                          io:city ?city.

  # Para hacer legible la información de la ciudad, busco el label
  # Filtro por Chapadmalal y me guardo los labels en una variable para el select
  ?city rdfs:label "Chapadmalal"^^xsd:string;
        rdfs:label ?cityLabel.

  # --------- INFORMACIÓN DE SUPERFICIE ---------
  # Obtenemos el espacio del terreno
  ?terreno rec:includes ?spaceLand.

  # Filtramos los espacios a que sean land (excluimos building sites)
  ?spaceLand a rec:Site;
              io:hasFeature ?featureTotalSpaceLand.

  # Filtramos las features a que sean superficie
  ?featureTotalSpaceLand a io:Superficie.

  # Obtenemos el blank node del detalle de la feature de superficie
  ?featureTotalSpaceLand io:hasDetail ?bnFeatureTotalSpaceLandDetail.

  # --------- INFORMACIÓN DE PRECIO ---------
  # Obtenemos los listings asociados a los terrenos y sus features de precio
  ?listings sioc:about ?terreno;
            io:hasFeature ?featurePriceListing.

  # Obtenemos el blank node del detail de las features de precio
  ?featurePriceListing io:hasDetail ?bnFeaturePriceListingDetail.

  # Obtenemos el blank node del ScraperValue de precio
  ?bnFeaturePriceListingDetail io:hasScraperValue ?bnPriceScraperValue.

  # Obtenemos valor, moneda y filtramos por tipo de precio base (excluimos fees)
  ?bnPriceScraperValue gr:hasCurrency ?currency;
                        gr:priceType "BASE"^^xsd:string;
                        gr:hasCurrencyValue ?value.

  # --------- CÁLCULO DE PRECIO POR M2 ---------
  # Obtenemos el blank node del ScraperValue de superficie
  ?bnFeatureTotalSpaceLandDetail io:hasScraperValue ?bnSpaceScraperValue.
  
  # Obtenemos los valores de superficie (tanto en HA como en M2)
  ?bnSpaceScraperValue gr:hasUnitOfMeasurement ?unit;
                      gr:hasValue ?spaceValue.
  
  # Filtrar solo las unidades que nos interesan (hectáreas y metros cuadrados)
  FILTER(?unit IN ("ha"^^xsd:string, "m²"^^xsd:string))
  
  # Calcular precio por m2 según la unidad:
  # - Si está en hectáreas: convertir a m2 (multiplicar por 10,000) y dividir precio
  # - Si está en m2: dividir precio directamente
  BIND(
      IF(?unit = "ha"^^xsd:string, 
          ?value / (?spaceValue * 10000), 
          ?value / ?spaceValue
      ) AS ?valueM2
  )
}
LIMIT 100
```