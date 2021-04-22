We can ofcourse build HTTP requests, parse JSON response and handle GraphQL querying by hand, but it is often not required or is cumbursome

Intead we can use existing infrastructure like Apollo, which has features like 
1. Directly send queries and mutations and not worrying about constructing HTTP requests or parsing response. Its a time saver
2. view-layer integration
3. caching
4. validating and optimizing queries based on schema

Popular options: Apollo (community driven), Relay (Facebook's)

## Directly Sending Queries and Mutations

GraphQL allows us to fetch and update data in a declarative manner, instead of the imperative manner in REST applications. i.e. instead of using fetch or similar JS clients and constructing HTTP requests on our own, we can just describe the data requirements and let apollo handle HTTP construction and response parsing at a lower level

## View Layer integrations and UI Updates

After the server responds, the data somehow needs to end up on the UI. Say for example in view, we can force update UI or just set the UI variables and let Vue update as a part of its cycle.

Here "layer" refers to the Functional Reactive Programming layer, formed by frameworks like Vuejs, react, or angular.


## Caching Query Results: Concepts and Strategies

Caching would avoid repeated requests to server and improve fluency of the user experience. The general approach is that data fetched is placed in a store and when a similar query is to be run, the values from store could be used instead of query reaching the server. This would be a naive approach if we used something like vuex. But with apollo, the data fetched is normalized as described here https://www.apollographql.com/blog/the-concepts-of-graphql-bc68bd819be3/

Check out [[GraphQL Concepts Visualized]] for more info on caching

## Build-Time Schema Validation and Optimizations

since schema has all information about the possible data structures, valid fields etc (all possible info required) the GraphQL client can validate and optimize queries at build time.

At build time all graphql code can be parsed and compared against the schema. This catches typos and other erros before deployment.

## colocating views and data dependencies

The tight coupling of views and their data dependencies greatly improves developer experience as there is no mental overhead about how the data ends up in right parts in the UI. This means we could potentially put the data dependencies code (think analogous to store code from vuex) and the UI code (UI rendering code) in the same file.

## Fluent GraphQL Clients

Fluent graphql clients allow us to write graphql queries as an object instead of a string that may not be very readable. they offer:

-   Strong typing
-   Single source of truth for type definitions
-   Autocompletion of queries.