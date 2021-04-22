Objectives

-   Differentiate examples of REST queries versus GraphQL queries.
-   Explain how GraphQL solves the over-fetching and under-fetching problem of REST APIs.
-   Discuss why GraphQL allows frontend and backend teams to work independently.

REST has some great ideas like stateless servers and structured access to resources.
It has strict specification, which most APIs don't fully adhere too, still they call themself restful (e.g. some resources may be stateful in many APIs). Also rapidly changing requirements on client-side makes it difficult due to the static nature of REST.

GraphQL was created to tackle the above problems.

Example:

A Blogging app that shows user info, all user posts and last 3 followers. This needs 3 endpoints, say

```
/user
/post
/follower
```
where `id` is passed through the auth token. This would work IMO.

If we do GET request to all 3 endpoints we do:

1. 3 requests to get 3 types of data
2. We get more data than what's required for the view (over fetching)

With graphql, we have a single endpoint, and the request would be something like:

```
query {
	User(id: "abc123") {
	name
	posts {
	    title
	}
	followers(last: 3) {
	    name
	}
	}
}
```

No Over and Under fetching


Opinion:

Products which have fast prototyping cycles in the client side will find lot of advantage with GraphQL. We are moving crucial logic of data resolution from backend into frontend. This could be a drawback in some applications, in which we would like more control over the data by placing it securely in backend.


Schema:

- strongly typed
- serves as a contract between client and server
- decoupling frontend and backend teams

