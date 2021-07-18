https://www.youtube.com/watch?v=9knwu87IfU8

From Vuex to Apollo State Management - By Natalia Tepluhina

Using vuex we generally dispatch actions that hit a rest API, in this action we set loading to true. Once the API call resolves, if successful, the result is commited and state mutates. On error, an error mutation is triggered, and state mutates.

This becomes redundant when using Apollo, since there will be two sources of truths and apollo client is now capable of state management.

With Apollo, when we send out a query, the state change upon success or failure is handled by apollo, so no dispatch -> Commit here.

In case of a mutation, we trigger a Graphql mutation, and add a `updateCache` function, which would be triggered upon success/error. This function will be responsible for state update.

What about local state?

Still apollo cache. We should be able to initialize local variables in apollo cache. The way to read this data (like getters) is to `query` for it (believe it or not) just like its from API.

```
query QueryName @client {
  field_1
  field_2
}
```

What about action + mutation?

In apollo, we have local mutations, which are like this:

```
mutation mutationName($var: Type!) {
  functionName(var = $var) @client
}
```

In short:
vuex -> Apollo

Mapped state -> Local query
Getters -> Local query + resolver
Actions + Mutations -> Local mutation + resolver

