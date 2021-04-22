Ref: https://www.prisma.io/blog/graphql-server-basics-the-schema-ac5e2950214e

This article describes how graphql.js is made and this kinda talks about how most popular implementations of graphql are made.

## Structure and implementation of GraphQL servers

So you have a schema, but how do we proceed further? A server implementation has 2 indepandant pieces to implement an API, the schema and the resolvers

Say we have a type in our schema such as

```graphql
type User {  id: ID!  name: String}
```

from a client, lets say we run a query such as 

```graphql
query {  user(id: "abc") {    id    name  }}
```

For this query to succeed, we need

1. A root type defined for querying. ie

```
type Query { user(id: ID!): User}
```

this becomes the entry point for the query 

2. Resolvers

In graphql.js (and in most implementations) we would find a GraphQLSchema object, which parses a schema and forms objects such as

```js
const UserType = new GraphQLObjectType({  name: 'User',  fields: {    id: { type: GraphQLID },    name: { type: GraphQLString },  },})

const schema = new GraphQLSchema({  query: new GraphQLObjectType({    name: 'Query',    fields: {      user: {        type: UserType,        args: {          id: { type: GraphQLID },        },      },    },  }),})
```

The SDL version is directly translated into JS. Next we need resolver functions, that carries the data access logic. 

**Resolvers implement the API***

GraphQL has a clear seperation between structure and behaviour. Schema defines the structure and resolvers define the behaviour. Theoretically, each field in the schema should have its own dedicated resolver. Each resolver knows how to fetch data for that field.

Anatomy of a resolver function:

Since each of the field in a type has its own resolve function, the User field in the above query also has one. In different languages and libraries, the link between SDL version and resolvers can be different, but it generally is based on naming convention of the resolver function and the name being passed as reference into the  `GraphQLSchema` object.

```js
const schema = new GraphQLSchema({  query: new GraphQLObjectType({    name: 'Query',    fields: {      user: {        type: UserType,        args: {          id: { type: GraphQLID },        },        resolve: (root, args, context, info) => {          const { id } = args // the `id` argument for this field is declared above          return fetchUserById(id) // hit the database        },      },    },  }),})
```

As you can see, the `User` field  in Query type has a resolver attached that hits the database.

Arguments:

1. `root` (also called `parent`): Graphql resolves _breadth-first_ i.e. level by level and the root argument is simply the result of the previous call intialized to `null` unless specified to be something else.
2. `args` the arguments that are passed into the query like `{id:"abc"}` to get user
3. `context`: An object that gets passed through the resolver chain that each resolver can write to and read from (basically a means for resolvers to communicate and share information). Think of the context object in AWS lambda.
4. `info`: An AST (Abstract Syntax Tree) representation of the query or mutation. You can read more about the details in part III of this series: [Demystifying the info Argument in GraphQL Resolvers](https://www.prisma.io/blog/graphql-server-basics-demystifying-the-info-argument-in-graphql-resolvers-6f26249f613a).

Query Execution:

![[query_execution.png]]

1.  The query arrives at the server.
2.  The server invokes the resolver for the root field `user` — let’s assume `fetchUserById` returns this object: `{ "id": "abc", "name": "Sarah" }`
3.  The server invokes the resolver for the field `id` on the `User` type. The `root` input argument for this resolver is the return value from the previous invocation, so it can simply return `root.id`.
4.  Analogous to 3, but returns `root.name` in the end. (Note that 3 and 4 can happen in parallel.)
5.  The resolution process is terminated — finally the result gets wrapped with a `data` field to [adhere to the GraphQL spec](https://spec.graphql.org/October2016/#sec-Data):

```json
{  "data": {    "user": {      "id": "abc",      "name": "Sarah"    }  }}
```

Now, do you really need to write resolvers for `user.id` and `user.name` yourself? When using GraphQL.js, you don’t have to implement the resolver if the implementation is as trivial as in the example. You can thus omit their implementation since GraphQL.js already infers what it needs to return based on the names of the fields and the root argument.

#### Optimizing requests: The DataLoader pattern

With the above execution pattern, we can end up with performance issues when doing nested queries. N+1 query problem is when the database is hit N+1 times instead of a single hit (or maybe 2).

Take for example:

```graphql
query {  user(id: "abc") {    name    article(title: "GraphQL is great") {      comments {        text        writtenBy {          name        }      }    }  }}
```

Here we need to do `fetchUserById` and suppose this object also has all the article IDs writtern by the user, then for each article ID we could do `getArticleById` and if each article object has commentIds in them, then for each commentId we do `getCommentById` So you can see here, how it would be a costly process.

To overcome this, we can use the DataLoader pattern -  the general idea is that resolver calls are batched and thus the database (or other data source) only has to be hit once.