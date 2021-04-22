stateless - A system in which the  past transactions do not influence present transactions. Think of a vending machine, that resets itself and doesn't respond based on your previous transaction. For any request, the response is based on the current state of the system. ^5c07f3

stateful - A stateful app is a program that saves client data from the activities of one session for use in the next session. The data that is saved is called the application’s state.

REST - A REST implementation has a set of web resources, with each resource identified by a URL and a request to such a resource typically using HTTP methods, responds with a text response. The request modifies the system (by means of an database operation, other actions) in a stateless way, ie past transactions do not influence present transactions (think google search (true to some extent) or think vending machine).

YAGNI - (you aren't gonna need it) Avoid implementing things that you might need in the future in favour of things you definitely need today.

greenfield projects - In [software development](https://en.wikipedia.org/wiki/Software_development "Software development"), a greenfield project could be one of developing a system for a totally new environment, without concern for integrating with other systems, especially not [legacy systems](https://en.wikipedia.org/wiki/Legacy_system "Legacy system"). Such projects are deemed higher risk, as they are often for new infrastructure, new customers, and even new owners. ^54b4c9