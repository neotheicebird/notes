1. SDL - Schema definition language provides the syntax to write schema of an API. GraphQL has its own type system that is used to define the schema of an API.

```
type Person {
  name: String!
  age: Int!
}

type Post {
  title: String!
  author: Person!
}
```
The `!` means required field. We have also declared relationship between two types.

In REST world we would do something like `GET /users/{id}` and we would expect a data in a certain format. Note, that the above data structure is static and if changed, the client and server has to come to an agreement for the client to work well. So the backend and frontend engineers should be in sync.

In GraphQL the structure of data returned is not fixed, instead it is flexible and the client decides what data is actually needed.

2. Query -  Unlike REST, in graphql the client has to be expressing about its data needs. It has to tell what it needs and in what structure it needs it in. For example,

```
{
  allPersons {
    name
	age
  }
}
```

Here `allPersons` is the root field (think of it like endpoint name), the block under root field is the `payload` of the query.

Queries can also pass arguments, for example

```
{
  allPersons(last: 2) {
    name
	age
  }
}
```
returns a list of 2 items max.

Nested queries

```
{
  allPersons {
    name
	posts {
	  title
	}
  }
}
```

3. Mutations - A way to create, update, or delete data. Generally the same structure as queries, but they start with the keyword `mutation`

```
mutation {
  createPerson(name: "Bob", age: 36) {
    name
	age
  }
}
```

the server responds such as

```
{
  "createPerson": {
    "name": "Bob",
	"age": 36
  }
}
```

Please note, the server is only responding with information you asked for after executing the mutation. You can ask for `id` since it would be assigned after exectution

```
mutation {
  createPerson(name: "Bob", age: 36) {
    id
  }
}
```

3. Subscriptions - Realtime connection to the server in order to get immediately informed about important events

```
subscription {
  newPerson {
    name
	age
  }
}
```

Start with the `subscription` keyword and think of `newPerson` as a SNS topic. When ever a new Person is created, a stream of data is sent to the client.

hottake: Pusher is good because they provide frontend libraries to incorporate subscriptions. But in graphql world, any graphql framework implemented would already have subscription baked into it and with just using SNS, we could achieve great results without some service like pusher. (Edit 09-05-2021: This is not true. While we can implement subscriptions with just Lambda, it is not optimal. We need a continuously running process. AWS AppSync seems like the way to go)

4. graphql schema

- defines capabilities of API by specifiying how a client can fetch and update data
- represents a contract between client and server
- collection of graphql types with specific root types. root types provide the entrypoint for the API. these could be query, mutation or subscription type.

For the query

```
{
  allPersons {
    name
	age
  }
}
```

In the schema we would need

```
type Query {
  allPersons(last: Int): [Person!]!
}
```

For mutation:

```
mutation {
  createPerson(name: "Bob", age: 36) {
    id
  }
}
```

the schema would need

```
type Mutation {
  createPerson(name: String!, age: String!): Person!
}
```
Note the Camel casing in schema definition and how the return type and argument types are clearly defined

similarly for subscription, in schema we would add

```
type Subscription {
  newPerson: Person!
}
```
