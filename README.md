# Observatory

## Wikidata Requests
First request done.
What must be improved:
* Display clearly the number of deaths and the place (maybe use a photo of the place)
* Not only use stampede (Q2165983) but also Q109905701 and Q20180685
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
