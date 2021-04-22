Possible security issues
-  malicious client could run a large query and potentially take down the server, or simply lead to huge bills. Abusive queries
-  abusive queries could be sent to try to extract sensitive information

There are a few strategies to mitigate these risks, but none of them is bulletproof. Its important to know what options are available and know their limits to help us make best decisions.

Objectives:

-   Discuss the strategies to protect your GraphQL server against malicious clients.
-   Discuss about timeouts, maximum query depths, and throttling.

## Timeout

The simplest strategy to defend against large queries is to set a timeout. It does not require the server to know anything about the query, all the server knows is the maximum time allowed for a query. This would also give a signal for frontend devs to construct queries that are optimal.

For example in case of serverless lambda based microservices, we could set the lambda to timeout after say 5 secs and give a timeout response (5xx)

Pros:

- Simple to implement
- Most strategies will still use a timeout as a final line of protection

Cons:

- Damage can already be done before the timeout kicks in
- Sometimes hard to implement. Cutting connectons after a certain time may result in strange behaviours.

## Maximum Query Depth

GraphQL schemas are often cyclic graphs, this means a client could craft a query like this one

```
query IAmEvil {
  author(id: "abc") {
    posts {
	  author {
	    posts {
		  author {
		    posts {
			  author
			}
		  }
		}
	  }
	}
  }
}
```

Just by reading the query, the server can estimate the depth of the query. The server can be configured with a maximum query depth of say 3 and the above query would become invalid.

Pros:

- Since the AST of the document is analyzed statically, the query does not even execute, which adds no load on your GraphQL server

Cons:

- Depth alone is often not enough to cover all abusive queries; for example, a query requesting an enormous amount of nodes on the root will be very expensive but unlikely to be blocked by a query depth analyzer

## Query complexity

Sometimes, the depth of a query is not enough to truly know how large or expensive a GraphQL query will be. In a lot of cases, certain fields in our schema are known to be more compute than others.

Query complexity allows us to define how complex these fields are and to restrict queries by implementing a maximum complexity.

```
query {
  author(id: "abc") { # complexity: 1
    posts { # complexity: 1
	  title	# complexity: 1
	}
  }
}
```

By assigning complexity=1 for all fields and by simply adding the number of fields queries we can find the complexity of the query and by setting a maximum complexity on the server, we can make a query pass or fail.

In a dynamic version, I think this can be done in the resolvers, by adding to and passing further the `complexity` variable through the context variable. At any resolver when the complexity exceeds maximum allowed, we can make the resolver return failure.
In fact, for each field we can set different complexity as we implement the resolvers.

Pros:

- Covers more cases than a simple query depth
- Reject queries before executing them by statically analyzing the complexity (by finding complexity by analyzing the query. This would add a complexity_analyzer function before starting to resolver.)

Cons:

- Hard to implement perfectly
- If complexity is estimated by developers: How do we keep it up to date? How do we find the costs in the first place?
- Mutations are hard to estimate: What if they have a side effect that is hard to measure, like queuing a background job?
- This is such a complex topic and doesn't seem to have a simple, obvious answer.

## Throttling

What if a client does medium sized queries that are accepted by our server, but does them frequently? We want to stop that behaviour. But unlike REST, where we can count the requests/unit-time to an endpoint from an IP address, its not effective in GraphQL case, as even few queries might be too much if they are large. 

Think of hitting a REST endpoint as a fixed query in GraphQL terms. So the backend developer has an idea as to how to restrict access to the information being asked for. They can add logic to punish unfavourable behaviour (like using throttling). But in GraphQL frontend has more power over the data, and its difficult to measure if the client behaviour is abusive. 

A good estimate is the server time to estimate how expensive a query is. With a good knowledge of your system, you can come up with a maximum server time a client can use over a certain time frame.

We also need to decisde on how much server time is added to a client over time. This is a classic "leaky bucket algorithm". Let's imagine maximum server time (bucket size) allowed is 1000ms and that the client gains 100ms of server time per second (leak rate)

Let's say a certain mutation takes exactly 200ms each time. If a client calls this operation more than 5 times within one second, then the client gets blocked until they have more available server time. After two seconds, the client can call the server again.

While this is a great (simple) way to throttle GraphQL queries, since naturally complex queries would end up consuming more time meaning you can call them less often and smaller queries can be called more often as they are fast to compute.

The difficulty is in expressing these constraints to the client, as there is no way for the client to know how much server time a particular query would cost and optimize for that.

Is there a better way?

## Throttling Based on Query Complexity

The complexity of a query can be made available for the client to read and therefore the throttling constraints can be estimated by the client. 

Just like time throttling, we can use the leaky bucket algorithm to reduce complexity consumed per call, and each passing second the client is idle, the complexity currency can be added back for the client to spend.

The GitHub public API actually uses this approach to throttle their clients. Take a look at [how they express these limits to users](https://developer.github.com/v4/guides/resource-limitations/).

Pros:

- Allows clients to have more flexibility in the queries they run
- Limits are easily communicated to clients when throttling based on query complexity

Cons:

- Clients need to know what queries they will run ahead of time to accurately estimate costs