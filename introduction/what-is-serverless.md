# What is serverless?

Firebase enables your team to use a "serverless" architecture. Serverless is a horribly misleading term, because it definitely involves servers. Lots of servers.

To understand serverless, let's start with the original application architecture: the monolith.

### Monoliths can be great... for some things

For years application have been developed as monoliths. A monolith is a single program that runs your entire website. Think Microsoft Excel. You install a single .exe file, and your computer uses Excel.exe to open and run .xslx files.

Web applications started off as monoliths. Your company would develop a single application, often written in .Net, Java or PHP, and run it on one server. When that server would get overloaded, they'd spin up a second server running a second copy of the application. And so on until hundreds of servers were running hundreds of applications.

Monoliths are often the easiest way to write an application, especially in the early days of a product; however, they can be truly evil to scale. So as an application grows, companies often migrate to a microservices architecture.

### Microservices scale much better

[Microservices](https://martinfowler.com/articles/microservices.html) are usually the next step after a company's monolith is overrun.

Microservices use HTTP calls to connect tens or hundreds of tiny apps together.

So instead of a single monolith that handles authentication, payments and sending emails to clients, a microservices architecture has one app for authentication, another for payments and another for filling your inbox with marketing emails.

Microservices allow us to scale up just the parts of our application that need it. So a single, tiny server may handle authentication, while ten servers send emails. You get to allocate resource where they're needed most, and the application architecture scales much better.

But you still have to manage infrastructure. You still have to spin up ten email servers, and you still need to make sure that those email servers stay in sync as customer data changes. It's manageable, but is there a better way??? Well... guess what! There **_sometimes_** is a better way!

### Serverless

The next progression in the app-fragmentation process is known as serverless. There are still plenty of servers, but you're not managing them any longer! Google, Amazon or Microsoft now gets to manage the servers, and you merely manage your own code.

Serverless is much more difficult from a service provider's point of view. The Google engineers who make your code run as if by magic are using extremely sophisticated tooling and programming methods to automatically scale your apps up and down.

The upside of serverless is that it saves the user—in this case you—significant time and resources. You deploy code to your service provider, and the service provider runs it and scales it up and down.

### Serverless is a collection of technologies

Most "serverless" web apps use a combination of technologies. These technologies include

- single-page applications (SPAs) written in JavaScript that provide the graphical user-interface to your users,
- a static file host that provides the JavaScript, CSS and HTML files necessary for the single-page application,
- a cloud-based database that the SPA connects to directly,
- and finally, a functions-as-a-service provider that runs secure functions on a server.

### Single-page applications

Single-page applications, also known as SPAs, are browser-based applications that render their own HTML and CSS. In olden times these pages were all rendered on the server. While server rendering is still the fastest way to get a page, especially for static content like blogs and news articles, most sophisticated web applications are now single-page applications.

It's a single-page application because the server renders just one page, and once the browser has that page, the JavaScript on the page manages all future renders. This takes the load off of the server and moves it onto the user's browser. Having the users do your processing is awesome. The load is distributed. Also, your users can get instant feedback as they make changes to the page.

Single-page applications are usually hosted on a static file host. Firebase Hosting is a static file host, and it's tuned just for that. It doesn't do any rendering of files. It doesn't serve dynamic content. It serves static JavaScript, HTML, images... whatever your single-page app needs to run.

### Functions-as-a-service

Functions-as-a-service is the next building block in our serverless architecture.

Single-page applications can do a lot of processing, but there are things that you ***NEVER*** want to do in a browser. For instance, you should never attempt to send an email in a browser. You shouldn't attempt to finalize a Stripe payment either.

While these things could potentially be done in a browser, these operations require secret API keys. For instance, finalizing a Stripe payment requires your secret Stripe API keys. If you were to send those keys to a browser it would be trivial for any user to inspect the network request and use your API keys to take over your account. 

Secure operations must be done on a secure server. But this architecture is called serverless! How on Earth does this work???

The answer is functions-as-a-service, or FaaS. Google's functions-as-a-service is called Cloud Functions. You may have heard of Amazon's Faas offering, AWS Lambda. 

This is a Firebase course, so naturally, we're using Cloud Functions.

Cloud Functions enables the developer to write discrete pieces of logic, known as functions, and export them to the Cloud Functions service for execution. Cloud Functions then listens to any one of a number of triggers. When these triggers are tripped, Cloud Functions executes your code with the trigger as the event context.

We'll get deep, deep into Cloud Functions in a future module. Just know that these triggers can be as standard as an HTTP call directly to the Cloud Function, or as Firebase-specific as a new Firebase Authentication sign up or a change in your Cloud Firestore database.

The point is that by using triggers and Cloud Functions, we can architect an app that scales from prototype to massive success without us lifting a finger.


