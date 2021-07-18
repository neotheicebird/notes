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

2. Make filter expressions into table attributes

In Dynamodb filtering + event sourcing isn't a great combination. For example, `Get all todos created by an user limiting to 20 items per page` would translate to a query such as `Query(user_id=:user_id and obj_type=:obj_type)` and later filter with `Fitler(latest=True)` or similar approach. The issue here is that  we cannot use the Dynamodb inbuild `Limit` parameter, as the limit is actually applied to the Query, before filtering. This means that if the query finds say 25 items with matching key conditions, the first 5 is then returned. But in the first 5, 1 or more items could be inactive items. This is a consequence of event sourcing. We want to filter out the inactive items before applying the `Limit`.

To do this we want more meta attributes, like `user_id/obj_type` which has both user_id and obj_type in one attribute, so that we can use a second sort key of `latest`. i.e. we are putting together 2 or more attributes into another attribute, to help filter during query time, instead of after querying.