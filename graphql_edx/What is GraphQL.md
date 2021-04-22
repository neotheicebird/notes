Objectives:
-   Define what GraphQL is.
-   Explain what the advantages of GraphQL over REST API are.
-   Present the history of GraphQL.

## What is GraphQL?

- New API standard invented by Facebook
- Declarative data fetching
- Single endpoint and responds to query with precisely what is asked

## How was REST developed?
REST was created in a time when the development speeds is no where closer to what it is today. 

## Why did facebook create GraphQL?
1. Increased moble usage, means we need more efficient data loading
2. Different frontend clients for various devices and users, means, it becomes difficult to maintain one API that fits all requirements. GraphQL might be a good bet for this scenario, as each client can access precisely the data it needs.
3. Fast development speed and expectation of rapid feature development. With REST, there is more dependency on the backend engineer when building or prototyping new features. This frontend-backend coupling slows projects down.

## What is an API? (By MuleSoft)

We are connected to the world like never before. We make reservations, book flight with the touch of a finger. API is the unsung hero.

API is the messenger that takes your request, tells the system what to do and gives the system response back to you. For example, in a restaurant, the waiter(API) takes your request and tells the kitchen(system) what to do and gives the response(food) back to you. 

Whenever you think of an API, think of a Waiter.

## A Study on REST (Rules of the REST game)

Representational State Transfer (REST) is a software architectural style that uses a subset of HTTP. A Web service that follows REST guidelines is called RESTful. Such a web service provides its web resources in a textual representation (think JSON), and allow them to be read and modified with a stateless protocol (think pure functions) and a predefined set of operations (HTTP methods?)

Web Resources used to be just documents or files identified by URLs. Today, its more generic and abstract and includes every thing, entity, actions that can be identified, named, addressed, handled or performed in any way on the web. Each resource has an URI which responds in HTML, JSON, XML etc. The response can also include hypertext links to related resources.

ELI5: A REST implementation has a set of web resources, with each resource identified by a URL and a request to such a resource typically using HTTP methods, responds with a text response. The request modifies the system (by means of an database operation, other actions) in a stateless([[Important Words#^56cfa7]]) way, ie past transactions do not influence present transactions (think google search (true to some extent) or think vending machine).

## Some points from a Nick Schrock blog on GraphQL

-   **Structured, Arbitrary Code:** Query languages with field-level granularity have typically queried storage engines directly, such as SQL. GraphQL instead imposes a structure onto a server, and exposes fields that are backed by _arbitrary code_. This allows for both server-side flexibility and a uniform, powerful API across the entire surface area of an application.
-   **Application-Layer Protocol:** GraphQL is an application-layer protocol and does not require a particular transport. It is a string that is parsed and interpreted by a server. (We would be using HTTP, but we can use gRTC with GraphQL too)
