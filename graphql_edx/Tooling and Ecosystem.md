## Introspection

For a client to discover the schema we can ask GraphQL server for this info by querying the `__schema` meta-field which is always available on the root type of a query per the spec.

```
query {
  __schema {
    types {
	  name
	}
  }
}
```

We would get a response such as 

```
{
  "data": {
    "__schema": {
	  "types": [
		  {
			"name": "Query" 
		  },
		  {
			"name": "Author" 
		  },
		  {
			"name": "Post" 
		  },
		  {
			"name": "__Schema" 
		  },
		  {
			"name": "__Type" 
		  },
		  ...
	  ]
	}
  }

}
```

We get Scalar types, Object types and also "introspection types" the dunder ones

We can do much more than getting name of types, say for example:


```
query {
  __type(name: "Author") {
    name
	description
  }
}
```

The more detailed the schema is defined the more easier it becomes for the frontend devs to introspect and be independant of the backend devs.

Documentation browsers, autocomplete, code generation are all consequence of these introspection types. 

## GraphiQL & graphql playground

We wont be introspecting using Apollo in our projects as that would be inefficient. GraphiQL is an IDE to work with the GraphQL API. You can run queries and introspect all you want before implementation.

Graphql Playground is a powerful IDE that uses GraphiQL under the hood.

## Introspection with Auth

One issue I found with GraphQL is that the implementation details of backend are more visible to the frontend and a lot of information are exposed to the client. We can mitigate this by exposing only the fields that are authorized for access by the client who is requesting. Say, `Post` type has sensitive fields which are accessible only by the post author, then such fields shouldn't be part of the introspection response returned by the server.

This is where GraphQL security comes in. I believe this is one place where REST shines or atleast it seems more secure. Why?

We authorize endpoints and with different permissions, an entire block of data becomes available or not to a particular user. In the simplest case, we define certain endpoints to be public and some private, then we can fine tune access based on authorization level in the tokens. I don't see how this is solved in GraphQL or assume it would become tedious.

## GraphQL Config

One configuration for all your graphql tools. We can add a `.graphqlrc` file that centrally defines how the IDEs etc are to be configured. Graphql Playground works will with graphql-config automatically

Can be installed as a dev dependency using npm or yarn

The search for config is done in places like `graphql.config.json`, `graphql.config.js`. `.graphqlrc`, `.graphqlrc.yaml` to name a few. So config can be locally defined per project, but it can also be globally defined and we don't have to think about it again.

But what is configured?

The simplest schema only points to source of graphql schema

`schema: ./schema.graphql`

This Could mean that by firing up the IDE from a particular project, that's projects' schema is loaded.


It is possible to load environment variables and use them in config

`schema: ${SCHEMA\_ENDPOINT:"http://localhost:4000/graphql"}`


Also we can pass headers in the schema request so that it is easier in the IDE


```
schema:
  - http://localhost:4000/graphql:
      headers:
        Authorization: Token
```
> Pay special attention to the indentation of the headers block.

We can specify documents for graphql operations and fragments

`documents: ./documents/*.graphql`