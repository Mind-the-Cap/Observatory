# Observatory

## Wikidata Requests
### First request
What must be improved:
* Display clearly the number of deaths and the place (maybe use a photo of the place)
* ~~Not only use stampede (Q2165983) but also Q109905701 and Q20180685~~ --> not needed as all items use stampede
* Find a way to show references

```SPARQL
#title:Stampedes repertoried on Wikidata
#Built on August 2022 by Capucine-Marin Dubroca-Voisin (work in progress)
#defaultView:Map

SELECT DISTINCT * WHERE {
      ?item wdt:P31/wdt:P279* wd:Q2165983.
      ?item wdt:P585 ?date.
      ?item wdt:P625 ?coords.
      ?item wdt:P1120 ?numberOfDeaths.
      ?item wdt:P1339 ?numberOfInjuries.
      ?item wdt:P276 ?place.
      ?item wdt:P17 ?country.
   }
}
```
[Link to the results](https://w.wiki/5ZtQ)

### Second request
* Shows clearly the number of dead and injured people
* Hides the coords in the views where they are not needed
* Shows a photo of the place where the disaster happened
* Has a link to Wikidata item
* Still needs to [use references](https://www.wikidata.org/wiki/Wikidata:SPARQL_query_service/queries#Properties_most_often_applied_to_references)

```SPARQL
#title:Stampedes repertoried on Wikidata
#Built on August 2022 by Capucine-Marin Dubroca-Voisin (work in progress)
#defaultView:Map{"hide": "?coords"}
#view:Table{"hide": "?coords"}
#view:Timeline{"hide": "?coords"}

SELECT DISTINCT ?coords (CONCAT (STR(?numberOfInjuries), " injured") AS $injured) (CONCAT (STR(?numberOfDeaths), " dead") AS $dead) ?placePicture ?placeLabel ?countryLabel ?date ?item ?itemLabel WHERE {
      ?item wdt:P31/wdt:P279* wd:Q2165983.
      OPTIONAL {?item wdt:P585 ?date. }
      OPTIONAL {?item wdt:P625 ?coords. }
      OPTIONAL {?item wdt:P1120 ?numberOfDeaths.
                ?item wdt:P1339 ?numberOfInjuries. }
      OPTIONAL {?item wdt:P276 ?place.
                ?item wdt:P17 ?country.
                ?place wdt:P18 ?placePicture. } #photo of the place where the stampede occurred (less aggressive than a photo of the stampede itself)
     SERVICE wikibase:label { bd:serviceParam wikibase:language "en" }
   }
ORDER BY DESC(?numberOfDeaths)
```
[Link to the results](https://w.wiki/5Zum)

### With colours
* Manually defined threshold to show different colours depending on the number of deaths
* Be careful: Mina (2411 deaths, 2015) is not shown because one event in a layer may hide some other in different layers
* Did not found how to specify the colour for each category (looks it's hard-coded) it does not match the importance (red for smaller stampedes)
* Changing the order in which the categories are defined does not change the color

```SPARQL
#title:Stampedes repertoried on Wikidata
#Built on August 2022 by Capucine-Marin Dubroca-Voisin (work in progress)
#defaultView:Map{"hide": "?coords"}
#view:Table{"hide": "?coords"}
#view:Timeline{"hide": "?coords"}

SELECT DISTINCT ?layer ?coords (CONCAT (STR(?numberOfInjuries), " injured") AS $injured) (CONCAT (STR(?numberOfDeaths), " dead") AS $dead) ?placePicture ?placeLabel ?countryLabel ?date ?item ?itemLabel WHERE {
      ?item wdt:P31/wdt:P279* wd:Q2165983.
      OPTIONAL {?item wdt:P585 ?date. }
      OPTIONAL {?item wdt:P625 ?coords. }
      OPTIONAL {?item wdt:P1120 ?numberOfDeaths.
                ?item wdt:P1339 ?numberOfInjuries. }
      OPTIONAL {?item wdt:P276 ?place.
                ?item wdt:P17 ?country.
                ?place wdt:P18 ?placePicture. } #photo of the place where the stampede occurred (less aggressive than a photo of the stampede itself)
      BIND(
      IF(?numberOfDeaths > 1000, "More than 1000 deaths",
      IF(?numberOfDeaths > 100, "More than 100 deaths",
      IF(?numberOfDeaths > 10, "More than 10 deaths",
      "Less than 10 deaths")))
      AS ?layer).
     SERVICE wikibase:label { bd:serviceParam wikibase:language "en" }
   }
ORDER BY DESC(?numberOfDeaths)
```
[Link to the results](https://w.wiki/5Zv4)

### With causes
* See [Help:Modeling causes](https://www.wikidata.org/wiki/Help:Modeling_causes) for context
* Troubles the table so mostly useful for CSV. Used this request to save a first export.
* Also added number of survivors
```SPARQL
#title:Stampedes repertoried on Wikidata
#Built on August 2022 by Capucine-Marin Dubroca-Voisin (work in progress)
#defaultView:Map{"hide": "?coords"}
#view:Table{"hide": "?coords"}
#view:Timeline{"hide": "?coords"}

SELECT DISTINCT ?layer ?causeLabel ?immediateCauseLabel ?contributingFactorLabel ?effectLabel ?immediateCauseOfLabel ?contributingFactorOfLabel ?coords (CONCAT (STR(?numberOfInjuries), " injured") AS $injured) (CONCAT (STR(?numberOfDeaths), " dead") AS $dead) ?placePicture ?placeLabel ?countryLabel ?date ?item ?itemLabel WHERE {
      ?item wdt:P31/wdt:P279* wd:Q2165983.
      OPTIONAL {?item wdt:P585 ?date. }
      OPTIONAL {?item wdt:P625 ?coords. }
      OPTIONAL {?item wdt:P1120 ?numberOfDeaths.
                ?item wdt:P1339 ?numberOfInjuries.
                ?item wdt:P1561 ?numberOfSurvivors. }
      OPTIONAL {?item wdt:P276 ?place.
                ?item wdt:P17 ?country.
                ?place wdt:P18 ?placePicture. } #photo of the place where the stampede occurred (less aggressive than a photo of the stampede itself)
      OPTIONAL {?item wdt:P828 ?cause. }
      OPTIONAL {?item wdt:P1478 ?immediateCause. }
      OPTIONAL {?item wdt:P1537 ?contributingFactor. }
      OPTIONAL {?item wdt:P1542 ?effect. }
      OPTIONAL {?item wdt:P1479 ?immediateCauseOf. }
      OPTIONAL {?item wdt:P1537 ?contributingFactorOf. }
      OPTIONAL {?item wdt:P9353 ?doesNotHaveCause. }
      BIND(
      IF(?numberOfDeaths > 1000, "More than 1000 deaths",
      IF(?numberOfDeaths > 100, "More than 100 deaths",
      IF(?numberOfDeaths > 10, "More than 10 deaths",
      "Less than 10 deaths")))
      AS ?layer).
     SERVICE wikibase:label { bd:serviceParam wikibase:language "en" }
   }
ORDER BY DESC(?numberOfDeaths)
```
