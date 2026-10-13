October 2026 

New triplestore / repo in OpenGDB filled with following query that was executed directly in the update window in OpenGDB

https://open-gdb.lod4hss.org/repositories/esw

This repo is to be used in workshops, etc.
 

The name of the data graph depends on Wisski behaviour, it is at the same time the base URI for new entities.

We use this name in all copies of elites suisses to avoid rewriting the queries each time with a different graph name

```
WITH <https://elites-suisses.lod4hss.org/resource/>
INSERT {
  ?s ?p ?o .
}
WHERE {
  SERVICE <https://lod4hss1.dh.unibe.ch/ontop/sparql> {
    ?s ?p ?o .
  }
}
```


Empty the graph:

```
CLEAR GRAPH <https://elites-suisses.lod4hss.org/resource/>
```
