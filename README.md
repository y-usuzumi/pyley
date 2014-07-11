pyley
=====

Python client for an open-source graph database [Cayley](https://github.com/google/cayley)

> Cayley is an open-source graph inspired by the graph database behind [Freebase](http://freebase.com/) and Google's [Knowledge Graph](http://www.google.com/insidesearch/features/search/knowledge.html). Its goal is to be a part of the developer's toolbox where [Linked Data](http://linkeddata.org/) and graph-shaped data (semantic webs, social networks, etc) in general are concerned.

##Sample

```python
  client = CayleyClient("http://localhost:64210/api/v1/query/gremlin")
  g = GraphObject()

  // Start with only one vertex, the literal name "Humphrey Bogart", and retreive all of them.
  query=g.Vertex("Humphrey Bogart").All();
  response = client.Send(query) #response.RawText contains raw JSON data
  
  # `g` and `V` are synonyms for `graph` and `Vertex` respectively, as they are quite common.
  query=g.V("Humphrey Bogart").All()
  response = client.Send(query)
  
  # "Humphrey Bogart" is a name, but not an entity. Let's find the entities with this name in our dataset.
  # Follow links that are pointing In to our "Humphrey Bogart" node with the predicate "name".
  query = g.V("Humphrey Bogart").In("name").All()
  response = client.Send(query)
  
  # Notice that "name" is a generic predicate in our dataset. 
  # Starting with a movie gives a similar effect.
  query = g.V("Casablanca").In("name").All()
  response = client.Send(query)

  # Relatedly, we can ask the reverse; all ids with the name "Casablanca"
  query = g.V().Has("name", "Casablanca").All()
  response = client.Send(query)
  
  # Let's get the list of actors in the film
  query = g.V().Has("name", "Casablanca")
                .Out("/film/film/starring")
                .Out("/film/performance/actor")
                .Out("name")
                .All()

  response = client.Send(query)
  
  # But this is starting to get long. Let's use a morphism -- a pre-defined path stored in a variable -- as our linkage
   film_to_actor = g.Morphism().Out("/film/film/starring").Out("/film/performance/actor")
   query = g.V()
            .Has("name", "Casablanca")
            .Follow(filmToActor)
            .Out("name")
            .All()
  response = client.Send(query)

  # Add data programatically to the JSON result list. Can be any JSON type.
  query = g.Emit({'name': "John Doe", 'age': 41, 'isActor': true})
  emitResponse = client.Send(query)
```

##Bugs
If you encounter a bug, performance issue, or malfunction, please add an [Issue](https://github.com/ziyasal/pyley/issues) with steps on how to reproduce the problem.
