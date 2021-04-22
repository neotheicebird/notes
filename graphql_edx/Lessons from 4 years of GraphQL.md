## Best practices for designing APIs

1. Naming matters: as soon as a client makes use of a field, it becomes difficult to change it. So make names that are self documenting and flexible enough for future proofing
2. Think in graphs, not endpoints
3. Describe the data, not the view: Starting from data modelling and creating a schema that is not influenced by the initial version of client. SImilarly in clientside make queries focus on data, rather than how it is represented
4. GraphQL is a thin layer: GraphQL is not intended to do authorization, authentication, caching, database query and optimization in itself. Handle these on your own and make the system resilient.

Graphql was made to address facebook's pressing issues in the past. Like mobile users were suffering with over-fetched data and so on. So if your problems align with old facebook's then great, graphql would be a great fit otherwise feel free to use other tools like different types of REST designs to solve your problem.

## Principles and Lessons

1. Solve a real problem: Set a priorities straight, identify the problems you are facing and pick tools that might help you solve it. Do not start from a pre-existing solution
2. YAGNI (You aren't gonna need it) Avoid implementing things that you might need in the future in favour of things you definitely need today.
3.  **Avoid "second system syndrome."** Fred Brooks talks about this in his classic software engineer book "The Mythical Man-Month." The risk is that when you build a new thing for the second time, you'll feel less cautious and more experienced, and end up over designing the whole thing. The best way to combat this is to have someone who can play devil's advocate for you and provide a second opinion. Be cautious!
4.  Encourage taking measured risks