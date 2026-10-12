Approaches:
* use a LLM for getting information [chat with Euria (12.10.2026)](https://euria.infomaniak.com/shared/01a1320e-33cc-71a8-9f98-2e4fc4d72114) 
* Inspect a card of a person, e.g. Dominique de Buman: https://www.wikidata.org/wiki/Q1238363

```sparql
### get the persons that had or have the position 
# of member of Swiss national council
SELECT ?person ?personLabel WHERE {
  ?person wdt:P39 wd:Q18510612.
  ?person rdfs:label ?personLabel .
  FILTER(LANG(?personLabel) = "en")
}
ORDER BY ?personLabel
limit 10

```

```sparql
### count the persons that had or have the position 
# of member of Swiss national council
SELECT (count(*) as ?num)
WHERE {
  ?person wdt:P39 wd:Q18510612.
}
ORDER BY ?personLabel

```
### Birth dates and places

```sparql
### get the birth dates and places
SELECT DISTINCT ?person ?personLabel ?birthYear ?birthPlaceLabel ?long ?lat
WHERE {
  ?person wdt:P39 wd:Q18510612.
  ?person rdfs:label ?personLabel .
  FILTER(LANG(?personLabel) = "en")

  # Get birth date
  OPTIONAL{?person wdt:P569 ?birthDate
  BIND(year(?birthDate) as ?birthYear)
  }

  # Get birth place
  ?person wdt:P19 ?birthPlace.

  # Get label of birth place
  ?birthPlace rdfs:label ?birthPlaceLabel.
   FILTER(LANG(?birthPlaceLabel) = "en")

  # Get geocoordinates of birth place
  ?birthPlace wdt:P625 ?geoCoordinates.
   BIND(STR(?geoCoordinates) AS ?wkt)
  # WKT: Point(<longitude> <latitude>)
  BIND(REPLACE(?wkt, "^Point\\((-?[0-9.]+) (-?[0-9.]+)\\)$", "$2") AS ?lat)
  BIND(REPLACE(?wkt, "^Point\\((-?[0-9.]+) (-?[0-9.]+)\\)$", "$1") AS ?long)
  
}
ORDER BY ?personLabel
limit 10

```
### Memberships and mandates with dates

```sparql
### get the statements and the dates
SELECT ?person ?personLabel (year(?start) as ?begin_year) (year(?end) as ?end_year)
WHERE {
  ?person  p:P39 ?statement .
  # member of Swiss national council
  ?statement ps:P39 wd:Q18510612.
  ?person rdfs:label ?personLabel .
  FILTER(LANG(?personLabel) = "en")

    # qualifiers of the statement
  # OPTIONAL {?statement pq:P580 ?start} 
  ?statement pq:P580 ?start 
  OPTIONAL { ?statement pq:P582 ?end }
}
ORDER BY ?personLabel
limit 10

```

```sparql
### get the statements and the dates of all memberships and function
SELECT ?person ?personLabel (year(?start) as ?begin_year) (year(?end) as ?end_year) ?oType ?oLabel ?o
WHERE {
  {
  # member of Swiss national council  
  ?person wdt:P39 wd:Q18510612.
  ?person rdfs:label ?personLabel .
  FILTER(LANG(?personLabel) = "en")
  ?person  p:P39 ?statement .
  ?statement ps:P39 ?o.
  ?o rdfs:label ?oLabel .
  FILTER(LANG(?oLabel) = "en")
  BIND('function' as ?oType)
  
    # qualifiers of the statement
  # OPTIONAL {?statement pq:P580 ?start} 
  ?statement pq:P580 ?start 
  OPTIONAL { ?statement pq:P582 ?end }
  }
  UNION
  {
  # member of Swiss national council  
  ?person wdt:P39 wd:Q18510612.
  ?person rdfs:label ?personLabel .
  FILTER(LANG(?personLabel) = "en")
  ?person  p:P463 ?statement .
  ?statement ps:P463 ?o.
  ?o rdfs:label ?oLabel .
  FILTER(LANG(?oLabel) = "en")
  BIND('member' as ?oType)
  
    # qualifiers of the statement
  # OPTIONAL {?statement pq:P580 ?start} 
  ?statement pq:P580 ?start 
  OPTIONAL { ?statement pq:P582 ?end }
  }
}
ORDER BY ?begin_year
limit 10

```

```sparql

```
