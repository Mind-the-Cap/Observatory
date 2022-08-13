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
      OPTIONAL {
      ?item wdt:P585 ?date.
      ?item wdt:P625 ?coords.
      ?item wdt:P1120 ?numberOfDeaths.
      ?item wdt:P1339 ?numberOfInjuries.
      ?item wdt:P276 ?place.
      ?item wdt:P17 ?country.
      ?place wdt:P18 ?placePicture #photo of the place where the stampede occurred (less aggressive than a photo of the stampede itself)
      }
     SERVICE wikibase:label { bd:serviceParam wikibase:language "en" }
   }
ORDER BY DESC(?numberOfDeaths)
```
[Link to the results](https://w.wiki/5Zum)
