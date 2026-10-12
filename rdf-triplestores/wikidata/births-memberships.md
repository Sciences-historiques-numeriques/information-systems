

## Members of the Swiss National Council
SELECT ?person ?personLabel WHERE {
  ?person wdt:P39 wd:Q18510612.
  ?person rdfs:label ?personLabel .
  FILTER(LANG(?personLabel) = "en")
}
ORDER BY ?personLabel
LIMIT 10
