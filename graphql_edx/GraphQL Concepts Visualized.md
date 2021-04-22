Ref: https://www.apollographql.com/blog/the-concepts-of-graphql-bc68bd819be3/

The true heart of GraphQL lies in what I think of as the application data graph.

## The application data graph

A lot of data can be represented as graph of nodes and edges. Nodes represent objects and edges represent relationship between the objects. 

![[data_graph.png]]

This is very similar to data modelling principles to design databases. For example consider this ERD (Entities relationship diagram). Here Entities are related to each other and based on these relationships database tables can be modelled.

![[erd_example.png]]

**GraphQL allows us to extract trees from the app data graph.**

### Traversing the Graph with GraphQL

lets take a query

```
query {
  book(isbn: "9780674430006") {
    title 
    authors {
      name
    }
  }
}
```

That responds with

```
{
  book: {
    title: “Capital in the Twenty First Century”,
    authors: \[
      { name: ‘Thomas Piketty’ },
      { name: ‘Arthur Goldhammer’ },
    \]
  }
}
```

Can be looked at in terms of a graph like

![[data_graph_2.png]]

This is actually clearer to me in terms of ERD, where `Book` and `Author` have a many-to-many relationship

In REST these relationships are not exposed directly to the client and client is at the mercy of the server, which handles these relationships and exposes endpoints with which the client can access data. The thing is, in most cases, these relationships are then remade in client side to populate the Views (more or less). So the contract that the API layer makes is costly as the data structure of the objects and their relationships gets broken.

For a query, GraphQL traverses through the data graph and creates a tree. This is done using breadth first search.

![[result_graph.png]]

## Caching results

The tree structure is extremely useful and is used in client-side caching.

Later on if some other section asks for information present in this tree, unless we absolutely need the newest data, this query can be resolved locally.

Apollo client assumes that the data trees created from queries that hit the server, are stable piece of information and unchanging. For cases when some piece of information that frequently changes, we can preven apollo from making this assumption using the concept of *object identifiers*. 

## Same path, same object

 ```
query particularAuthor {
  author(name: "Thomas Piketty") {
    name
    age
  }
}

query authorAndBook {
  book(isbn: "9780674430006") {
    title
  }  author(name: "Thomas Piketty") {
    name
    age
  }
}
```

Consider the two queries above, you can see that the after the first query is executed, in the second query there is redundancy and the query can be optimized as part of the information is cached. i.e. the Author part need not hit the server. Apollo client uses this kind of logic to remove parts of query based on data present in cache. Don't be alarmed if the server receives optimized queries not same as ones you sent.

Again if this kind of assumption won't work you can override this default caching entirely using *forceFetch* option

### Use _object identifiers_ when the path assumption isn’t enough

Sometimes just following query paths to find same pieces of information may not be enough, because queries can get complicated or the relationship between objects can be weird. In such cases we can help Apollo identify same piece of information in order to create a more optimized cache and save time/effort.

Here is an example:

```
query {
  author(name: "Arthur Goldhammer") {
    coauthors {
      name
      id
    }
  } 
}
```

Returns information about an author by name (which the backend will know how to handle)

Another query

```
query {
  author(id: "5") {
    name
    id
  }
}

```
returns author of id 5. Let's assume that Author with name "Arthur ..." and author with id=5 are coauthors of some book (which means we already have the information of author id=5 in cache but apollo doesn't know its the same piece of information). To solve that issue, Apollo Client supports a second concept: object identifiers. Basically, you can specify a unique identifier for any object you query. Then, **Apollo Client assumes that all objects with the same object identifier represent the same piece of information**.

![[obj_identifier.gif]]

Object identifiers could be something like "ID" of the object + ":typename". like "Author:5", "Book:33" or something.

### Keeping query results consistent

Lets say after caching a few results with for example a Book with ID=5, we do a forced query that hits the server and the result of Book.id=5 is changed. Then we would expect the result changed in all places it is used. Which is exactly what apollo does. guarantee that Apollo Client provides: **If any of a watched query tree’s nodes change in value, the query will be updated with the new result**.

![[result_change.gif]]

Sometimes it doesn’t make sense for your application to have object identifiers for everything, or you might not want to deal with them directly in your code but still need particular bits of information in the cache to be updated. This is why we expose convenient but powerful APIs such as _updateQueries_ or _fetchMore_ that let you incorporate new information into these query trees with very granular control.

### Summary

The backbone of application lies in app data graph. Caching was hard in REST world, since data fetching was application specific and we couldn't find easy, consistent abstractions. But in GraphQL, this is easier to be automated.

If you understand five simple concepts, you can understand how reactivity and caching (i.e. all the magic that makes your app fast and slick) work within Apollo Client. Here they are, restated:

1.  GraphQL queries represent a way to get trees out of your app data graph. We call these _query result trees._
2.  Apollo Client caches _query result trees_. To do this, it applies two assumptions:
3.  _Same path, same object_ — the same query path usually leads to the same piece of information.
4.  _Object identifiers when the path is not enough_ — if two results are given the same object identifier, they represent the same node/piece of information.
5.  If any cache node involved in a query result tree is updated, Apollo Client will update the query with a new result.