Possible Takeaways:

1. An application can be broken down into a set of services, as a dev I just have to figure out how to make each service alive and handle the relationships between the services
2. Development is a game of tradeoffs - Each tech has a set of features, our job is to figure out the right tool for the job
3. Think of it like a puzzle, but not one with only one right answer.
4. How to think like a developer

Possible flow of presentation:

0. About me
1. Let students know what they can expect to learn from this talk:
	1. What kind of problems can we solve using web apps
	2. How were such apps made in the past, pros and cons
	3. How are modern web applications made (micro-services model)
	4. Why you can start building one today
2. Talk about the "problem" of web apps, generally what it entails, what is expected - *GIF*
3. How were such apps made in the past, legacy systems, the pain that is involved in maintaining them, downtime, scaling issues - *GIF*
4. Why is the ecosystem different today? (talk about pain points in maintaining traditional apps) - *GIF*
5. Breaking up applications into "services" is the central idea in the process of today's app development - If you take the analogy that a system (like uber) is like an organism (like human body), then its made of organs each performing a set of tasks its aware of. But there emerges a synergy, a natural intelligence, that keeps everything working together. *GIF*
6. Introduce some services like authentication, data storage, business logic, emails, sms
7. Introduce event based microservices - talk about synergy - end with how do you build one? *Synergy GIF*
8. Two worlds of microservices - Serverless vs Containers - not opposing, works together - If this is the modern paradigm, what is the role of a software developer
9. Role of a webapp developer - Full Stack dev, DevOps - interchangebale, and no clear border with the expected set of skills - *GIF*
10. How can you be one today? - You are already using a lot of these services and are well into the learning curve - Code or NoCode doesn't matter, pick your battles, pick the right tools for your use case - How about starting NoCode, understanding the limitations, then go code?
11. Let's build something - Build something nocode, and add a bit of code - low code demo
12. Thanks

Extras:

- Keep a strong GIF game, that is in Tamil/Indian context
- Ask some raise the hands questions


Most Common Services
===
Client
API Gateway
Authentication
Data store
SMS
Email
Websocket
Message Queue
payment/home/prashanth/sa_engg_talk/gatsby-config.js

Demo Ideas
===
Google Sheets, Google Auth, Zapier, Simple frontend using Vue

The "Problem" of web applications
===
In today's world web applications are everywhere. They are used to solve all kinds of problems, like customer-business interaction (?) apps like Amazon, Dunzo. Business apps (name a few). Basically they act as  a bridge between humans and computers. Web apps make computers perform a set of tasks and help businesses serve customers.


What is the problem statement for a demo?

Submit your favourite talk for a community to enjoy

Euro cup 2020


Services I know:

- 
- email: SES, MailChimp
- mongodb
- stripe

Buzzwords:

Microservices, Microapps, 



*say something funny*

Use MDX to build your talk

https://github.com/jxnblk/mdx-deck


Notes:

0. About me:

Hi, I am Prashanth, I am a Full Stack web developer and I work freelance. I am currently involved in a project with a Canadian Startup called DevShed, and we are building Enterprise web apps to solve a problem in Shipping and Trucking industry. I am also building other apps and I am working with a few more clients.

I live here in Chennai, My dream day would be silent, maybe rain in the background, a tea in my hand and I am reading a book by some master like Lao Tzu, Marcus Aurileus or Khalil Gibran.

1. What can you expect from this talk? You are likely to learn about
	1. What kind of problems can we solve using Web Applications?
	2. How were web apps made in the past?
	3. How are they made nowadays? Why did we change the approach?
	4. Why you can start building one today?
2. What are web apps?
		An application is a software that helps humans accomplish a set of tasks. 
	
		A web application is one that runs on a server somewhere, and we interact with it using a device like our mobile phone or a computer, most likely through a web browser.
		
	1. What kind of problems can we solve using web apps?
	
	Engineering is about problem solving right? So what kind of problems can we solve by making web apps?
	
	Generally businesses interact with users using apps. The users can be customers, experts, general public.
	
	Ecommerce (Amazon, Flipkart)
	Communication (Slack, Whatsapp, Google Meet)
	Business Productivity (Google suite, Trello, JIRA)
	Accounting (Quickbooks)
	Travel (Airbnb, Ola)
	Social Media (instagram, facebook)
	
	They are everywhere, you get the gist. *So many apps* [https://media.giphy.com/media/p9G0FYn3Wr1Tjz7VbL/giphy.gif]
	
	3. How is a web application engineered?
		1. Take any typical app and we immediately see that its a complex system, which takes care of a lot of engineering "responsibilities". Ones like 
			1. Authentication - We signup and login by email, sometimes we login by our mobile number and use an OTP. All this logic has to be handled somewhere
			2. Data Storage - We have a whole variety of databases each with its own set of good and bad, tradeoffs.
			3. Scheduled Tasks - Calendar events, Reminders
			4. 
	4. How were such apps made in the past, pros and cons

		A "monolithic" application is one in which there is no clear "seperation of responsibilities". What does it mean?
		
		Not long ago, we used to make servers that solved every sub-problem of a web app! Be it Authentication, Data storage, Business logic, scheduled execution of tasks, and so on. These kind of applications are said to have a "Monolithic Architecture" a term that was coined recently as we moved towards a different architecture for making web applications.
		
		
	
References:

1. https://emilyriederer.netlify.app/post/writing-a-tech-talk/