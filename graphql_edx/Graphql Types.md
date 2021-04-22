There are two different types:

1. Scalar types: String, Int, Boolean, ID, Float
2. Object types: They have fields that express the properties of that type and are composable. Examples of object types are the **User** or **Post** types we saw in the previous section.

We can also define our own Scalar and Object types. For example, `Date` could be a Scalar type provided we define how the type is validated, serialized and deserialized.

## Enums

```
enum Weekday {
  MONDAY
  TUESDAY
  WEDNESDAY
  THURSDAY
  FRIDAY
  SATDAY
  SUNDAY
}
```

these are special types of Scalar types

## Interface type

interface can describe a type in an abstract way. It allows us to specify a set of fields that any concrete type which implements the interface needs to have. For example,

```
interface Node {
  id: ID!
}

type User implements Node {
  id: ID!
  name: String!
  age: Int!
}
```

## Union type

A type that is a collection of other types

```
type Adult {
  name: String!
  work: String!
}

type child {
  name: String!
  school: String!
}
```

Now to define a `Person` type to be an union of `Adult` and `Child` we can do

`union Person = Adult | Child`

But how does this work on the query side?

```
{
  allPersons {
    name
	... on Child {
	  school
	}
	... on Adult {
	  work
	}
  }
}
```


