Objectives:
-   Present the three different kinds of architectures that include a GraphQL server.
-   Explain how GraphQL can integrate third party or legacy systems.
-   Understand resolver functions.
-   Describe how GraphQL can make front-end development easier

GraphQL is only a specification, so different projects could interpret or implement it in different ways and therefore there are many architectures in the ecosystem.

Three common architectures:

- GraphQL server with a connected database
- GraphQL server to integrate existing system
- A hybrid approach with connected db and integration of existing system

1. Graphql server with connected db

* Often used in Greenfield Project ([[Important Words#^54b4c9]])
* uses single web server that implements GraphQL
* server resolves queries and returns response

2. Integrating with existing systems

* Used to unify existing systems like Legacy systems, existing microservices and many different APIs
* Has a single webserver that takes responsibility of taking with other systems and resolve queries

3. Hybrid approach

* Used in a webserver that is connected with a database
* But this webserver also *knows* how to talk to third party APIs or legacy systems and resolve queries


## Resolver functions

* Graphql queries/mutations are made of a set of fields
* Each field has a resolver function assigned to it
* The resolver's job is to get data for that field when asked
* First resolver to be implemented is for the *root* field and then deeper levels are resolved starting from the root. The resolution order is breadth-first search.

## GraphQL Clients

* GraphQL is great for frontend devs as data handling complexity is pushed to the server-side, which generally has more computation power
* It becomes easier for front-end to interact with API using abstractions
* Move from imperative data fetching (REST) to purely declarative one (GraphQL). ie. in REST, a HTTP request is constructed, the respose is received and parsed, stored locally and displayed in UI. But in GraphQL, a data required is described, and displayed in UI. Everything in between is handled by graphql client. Agreed, most of these steps is handled by our abstractions with REST as well, still I am used to sufficient handcoding to handle HTTP requests. 