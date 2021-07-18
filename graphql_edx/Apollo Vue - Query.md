## Querying

While there is atleast 2 popular ways to do querying, I prefer the way of keeping querying parts in component's apollo object, resembling vue methods. This keeps the markup clean, and not cluttered with query related code.

I first started with using their second option which is to use `<ApolloQuery>` tag in markup. But this lead to seperation of concerns, with data logic in two areas, one in markup and other in methods, and I hated this pattern. Let's explore the `apollo` object based patterns.

### Simple query

```
<template>
  {{ todos }}
</template>
<script>
import gql from 'graphql-tag'

export default {
  name: 'ExamplePage',
  data() {
  return {
  		todos: []
  	}
  },
  apollo: {
    todos: gql`
		todos(last: 5, 
		 ) {
		 items {
		 	name
		 	entity_id
		 }
		 last_evaluated_key
		 }
	`
  }
}
</script>
```

This is a simple query where the query is treated as a apollo option, i.e a property in apollo object. Works for simple straightforward queries (like ones without arguments for example)

An output such as `{ "items": \[ { "name": "My first todo", "entity\_id": "3cf5074b62254f97851c470e4d96fa52", "\_\_typename": "Todo" } \], "last\_evaluated\_key": null, "\_\_typename": "TodosOutput" }` goes into `todos`. 

### Name matching

in the last query, we made sure that `todos` is fetched from the backed and saved in local variable `todos`. In fact, apollo made sure this works, we didn't. What if we named our local variable `items` instead? will this have worked, NO. How to make it work?

```
apollo:{
  items: {
    query: TODOS, /* last query with todos is in this variable */
	update: data => data.todos.items
  }
}

```
As you can see an update property is used to decide what goes into `items` if its not obvious as in previous case. 

## Passing query params

```
apollo: {
  todos: {
    query: TODOS,
	variables: {
	  last: 5
	},
	```
	// Additional options here
    fetchPolicy: 'cache-and-network',
  }
}
```

Where `TODOS` could be something like:

```
gql`
query Todos($last: Int!) {
  todos(last: $last) {
    items {
	  name,
	  entity_id
	}
  }
}
```

Note that, we can pass additional options like fetchPolicy, pollInterval to change the behaviour of the query.

## Loading state

we can use the query loading state variable to add a loader in the website. It can be found at:

`this.$apollo.loading` for any query that is loading
 or 
`this.$apollo.queries.todos.loading` for a specific query load

## Option function

We can make the apollo option a function, like

```
apollo: {
  todos() {
    // write code you want here
    return {
		query: TODOS,
		variables: {
		  last: 5
		}
	}
  }

}
```

This function is called once when the component is created. We can use this place to write "pre-query" logic

## Reactive queries

What if I have two 	queries that both fetch `todos` in different ways?

```
apollo: {
  todos: {
    query() {
	  if (this.useFirstQuery) {
	    return TODOS_1
	  } else {
	    return TODOS_2
	  }
	},
	update: data => data.todos
  }
}
```

I think, any propery inside apollo can be made into a function with return if we want to add more logic to it !?

## Reactive params

Guess what, we make the `variables` option of a query into function

```
apollo: {
  todos: {
    query: TODOS,
	variables() {
		return {
		  last: this.calculateItemsCount()
		}
	  }
	}
}
```

## Skipping query

We can add a `skip` option to a query and assign it to `true` to skip it. ofcourse this can be made into function as well.

```
apollo: {
  todos: {
    query: TODOS,
	variables {
	    last: 5
	  }
	},
	skip() {
	  return this.skipTodosFetch
	}
}
```

