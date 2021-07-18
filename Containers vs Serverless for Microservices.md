Key factors in evaluating the stacks:

1. Reduce dependency on a single cloud provider
2. Higher speed of development
3. Tech that has bigger market to hire developers
4. Reduce net costs that are likely to be billed
5. High Performance

Architecture:

The software architecture appropriate for patterns based on the Trucking app and DIY API builder app is **Microservices** based. We would be needing services like the following, which are loosely coupled to each other and play together to form the whole software:

1. NoSQL Database (e.g. MongoDB, DynamoDB)
2. Functions as a service (FaaS) - Handles events (API requests, Queue events), execute business and data access logic
3. Message Queues (e.g. Kafka, SQS) - User generated events can be processed through queues, Emails to be sent can be queued, PDF processing can be queued etc
4. Servers (Optional, e.g. ExpressJS) - Optionally, if the application has to be long standing, they can be run in servers using frameworks like Express

The main contenders:

1. Serverless stack - AWS Lambda, SQS, Dynamodb and other AWS based services
2. Containers stack - Docker, Express/Flask server, Kafka/Redis, MongoDb and other services
3. Mixed stack - A Express/Flask server with "monolithic" code, and using cloud resources like mongo, kafka, etc as needed

Serverless Stack

Pros - 
1. Pay only for what's used, very cheap to start with - Lambda, Dynamodb, SQS are all pay per use
2. Elasticity - Scaling is automatic and doesn't need any seperate maintenance
3. Fast Development - With all dev efforts focused on business logic, the development is fast
4. Plays well in microservices architecture - We can use services outside and in AWS to build the products
5. Easy provisioning of resources - With AWS's cloudformation and Serverless framework, its easy to define resources needed and provision them on cloud

Cons -
1. Cold start - if the app doesn't get constant traffic, some users may feel latency
2. Vendor Lock-In - This can be partially mitigated by using [serverless framework](https://www.serverless.com/), as we can modify the config files and move to other cloud providers for some services. This still breaks some of the services.
3. Doesn't grow well with complexity - If the requirements get complex, it becomes difficult to handle, so we would start to solve the problems by introducing lot more resources and complicating the backend.
4. Short running - AWS lambda has a max of 15 minute execution time. It might not be optimal for cases where a logic needs to run longer, say for processing a large PDF.
5. Limited Memory - While mostly there is enough memory available, there could be cases for which we need large memory, like video processing, which cannot be done in serverless and leads to increased complexity of the application
6. Difficult to develop locally, this slows down developers

Container Stack:

Pros - 
1. Flexible, Portable and Vendor agnostic
2. Full control over the application - We are not limited by cloud provider's restrictions (like memory, timeout, languages supported etc)
3. Lots of options - With loosely coupled services in place, its easy to move from one technology to another
4. Grows with complexity - Even when the complexity of the problem grows, the complexity of the software is manageble.
5. Easy local development - What we see locally is mostly what we can expect after deployment and this saves developer time

Cons -
1. With great powers come great responisibilty - we need DevOps and Devs who pick the right version of a technology, make sure any new security patches are applied, follow the images on DockerHub
2. Expensive - Containers run like servers and are billed per month, so they become considerably expensive for use cases with variable traffic.
3. Scaling - Unlike serverless, scaling has to be managed and any issue has to be handled by DevOps. Scaling is slower and the developer time that is spent here, can be seen as wasted for a startup
4. Monitoring - With more and more services added to the project, monitoring the status and metrics of different services become difficult
5. Persistance - Data has to be stored in cloud or other reliable locations, storing in containers is not ideal.

Mixed Stack

Pros - 
1. Quick development for small teams
2. Focus on building business logic
3. Good middleground - Easy to transition to serverless or containers if needed
4. Use both serverless and containers - As the complexity of apps grow, we can pick and use both kinds of technologies. This mixed strategy can be of great advantage
5. Easy local development - Simple containerization strategies can be used to make local development easy and reliable
6. Economic

Cons - 
1. Mandatory transition - We need to transition to (or gradually include) scalable technologies as the app gains traffic, we are just setting up a process of solving business need first and in future work on packaging for cloud

Takeaway

1. Serverless is better when speed of development and cost minimization is important
2. Containers are best for complex, long running projects
3. Serverless and Containers are not opposing stacks and can be used with each other

JS vs Python for backend

- Python is pretty popular for a few years and the community has been great
- Myself being a Python dev for many years, I would be lot more efficient using it over JS

Express Vs Flask

- Both of them are microframeworks. Express uses JS, Flask uses Python
- Both are easy for a new dev to jump in

My Stack Recommendation

Mixed Stack could be the best way to go for the following reasons:
- Fast development
- Easy for developers to jump in as the stack choices are popular
- works great locally (essential to have fast development)
- Flask can be deployed to AWS Lambda + API Gateway using Zappa to start with, this solves the scaling issue (Also easy to migrate to containers in future if needed)
- Kafka as a event log is battle tested and can be containerized locally and deployed on AWS Oregon Region (close to Vancouver Island)
- MongoDB is a great overall NoSQL db
- Auth0 for authentication (since this is an enterprise app, the number of users might not grow to millions)

Notes:

1. Database should be outside containers (like a mongodb instance on cloud, Kafka instance on cloud)
2. 

Refs:

[1 - Containers Vs Serverless](https://www.thorntech.com/containers-vs-serverless/)

Kafka - 

Flask for API, what about other events?

Kind of problems:

1. REST API requests from external source  or UI
2. Email with PDF triggers and processes PDF (we can use REST)
3. SMS response trigger lambda (Twilio + REST)
4. S3 file change trigger lambda
5. Async/scheduled lambda trigger

Only MongoDb for event sourcing (https://www.slideshare.net/dbellettini/cqrs-and-event-sourcing-with-mongodb-and-php)
MongoDb - How to avoid concurrency violations?

Where to place kafka?
kafka + serverless (possible, but not beautiful)
flask + kafka

Serverless framework is a great fit, but vendor lock-in.

Why is zappa great? https://techbeacon.com/enterprise-it/serverless-development-evolves-why-my-team-uses-zappa

Some of the features all of these stacks have:

1. Event based computation
2. Respond to web requests (REST API implementation)
3. Scheduled execution of tasks
4. Process data in async fashion (e.g process PDF, prepare customer bills, charge customer in background)
5. Event sourced data storage
6. Respond to non-UI based events (like on SMS receive, on receiving emails)


Flask, MongoDb, S3/minio, RabbitMQ


- Flask is a stable and popular microframework (just like ExpressJS in design)
- With Zappa, flask can be served as a serverless app on AWS Lambda
- We can move away from AWS and deploy the REST API part in container model easily
- Easy for devs of all levels to jump in and easy to hire devs




