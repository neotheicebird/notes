
Refer to [[Apollo Vue - Query]] for a nice explantion of using apollo vue queries before starting mutations

## Mutations

We can use `this.$apollo.mutate()` to send a Graphql mutation

This can be done in vue methods, like for example:

```
methods: {
  addTodo() {
    const name = this.todoName
	
	this.todoName = '' // clears the UI input component, its snappy
	
	this.$apollo.mutate(
	{
		mutation: ADD_TODO, // mutation loaded as var
		variables: {
		  name: name
		},
		// update cache with results
		// it is the devs job to keep cache updated, otherwise
		// the mutation would suceed, but we wont see UI changes
		// as the UI elements linked by a query wont see any difference
		
		update: (store, {data: { name, entity_id }}) => {
		  const data = store.readQuery({query: TODOS})
		  
		  data.todos.push({
		    name
			entity_id
		  })
		  
		  store.writeQuery({query: TODOS, data})
		},
		// Optimistic UI
        // Will be treated as a 'fake' result as soon as the request is made
        // so that the UI can react quickly and the user be happy

		optimisticResponse: {
		__typename: 'Mutation',
		addTag: {
		  __typename: 'Tag',
		  entity_id: -1,
		  name: name,
		},
	  },

	}).then((data) => {
	  // I think we would have to update our local variables with
	  // right entity_id and name once the server responds
	  console.log(data)
	}).catch((error) => {
	  // in case of error, remove the last item
	  // show message to user
	})
  }
}