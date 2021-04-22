GraphQL is awefully (awesomely) front-end focused API tech. That doesn't mean the backend development is painful. Infact graphql enables backend devs to focus on describing the data available rather than implementing and optimizing specific endpoints.


By the end of this section, you should be able to:

-   Explain how GraphQL resolvers execute and resolve queries.
-   Explain how batched resolving improves the performance of your GraphQL API.
-   Discuss how the compiler approach helps with reducing multiple database hits.

## GraphQL Execution

The algorithm used to convert queries to result is quite simple. the query is traversed field by field, executing "resolvers" for each field.

The execution of a query starts with the query type (root) and goes breadth-first. At the end, the execution algorithm puts everything together into the correct shape for the result and returns that.

One thing to note is that most GraphQL server implementations will provide “default resolvers” - so you don’t have to specify a resolver function for every single field.

## Batched Resolving (client side)

The previous execution algorithm discussed is naive. Because for a resolver that hits the backend, the backend might be hit multiple times unnecessarily.

For example, for a query

```
query {
	posts {
	    title
		author {
		    name
			avatar
		}
	}
}
```

Here since Posts would be a list of items, for each item the author object would be retrieved. This does not account for the chance that multiple posts could have the same author and extra hits are made on the db.

To solve this, We can wrap our fetching function in a utility that will wait for all of the resolvers to run, then make sure to only fetch each item once:

That is all all fetches to a queue, removing duplicates and then start fetching.

Can we do better, if the API supports batched requests, we can do only one fetch.

The above strategies can be implemented using an utility called DataLoader in js.

## Compiler approach (client side)

Batching of resolvers solves performance problems to a large extent. It reduces multiple hits to the database. But, even with batching, there would still be multiple hits to the database depending on the depth of the query.

The compiler approach lets you map a GraphQL query of any depth to one database query.

## Fragments

Improving structure and reusability of graphql code. A fragment is a collection of fields of a specific type

```
type Query {
  allUsers: [User!]!
}

type User {
    name: String!
	age: Int!
	email: String!
	street: String!
	zipcode: String!
	city: String!
}
```

here we can create a fragment for address such as 

```
fragment addressDetails on User {
    name
	street
	zipcode
	city
}
```

This can be used in query to make it shorter such as

```
query {
    allUsers {
	  ... addressDetails
	}
}
```

Notice the spread operator, that makes sure a query such as following is executed

```
query {
    allUsers {
	  name
	  street
	  zipcode
	  city
	}
}
```

## Parameterizing Fields with Arguments

We can add arguments to fields in schema. Each such argument should have a name and a type, they can also have a default value

```
type Query {
  allUsers(olderThan: Int = -1): [User!]!
}
```

a sample query that can work with the above schema

```
query {
  allUsers(olderThan: 40) {
    name
	age
  }
}
```

## Aliases

In graphql we can send multiple queries in a single request. But...

```
{
    User(id: 1) {
	  name
	}
	User(id: 2) {
	  name
	}
}
```
this would fail as Graphql doesn't know how to structure the result. Here comes aliases to the rescue


```
{
    first: User(id: 1) {
	  name
	}
	second: User(id: 2) {
	  name
	}
	
}
```

this leads to the response

```
{
  "first": {
    "name": "Hasan"
  },
  "second": {
    "name": "kenny"
  }
}
```
Infact this is the only way to achieve the results