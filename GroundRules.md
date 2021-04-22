1. Always paginate getAlls

Github graphql API makes sure that any query to get multiple items always has the `last: Int` argument in the query. We cannot run queries without that, say

```graphql
{
  todos {
    name
  }
}

```
Won't simply work. We would have to do `todos(last: 5)`

If we make this a principle and make sure, from storage layer, through repo layer to resolver we implement this, it could never become a performance hurdle.

in storage, we should never be able to do `get_objs('todos')`, we should only be able to do something like `get_objs('todos', last=10, from="key")`. ergo, pagination is built-in.