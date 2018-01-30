# Cloud Firestore: Introduction

Cloud Firestore is the successor to both Cloud Datastore and the original Firebase Realtime Database.

Cloud Datastore already powers massive web applications, and the Realtime Database proved how amazing direct browser-to-database interactions can be. Cloud Firestore combines the scalability and reliability of Cloud Datastore with the awesome client libraries of the Realtime Database to create a massively scalable, easily-to-develop cloud database platform.

### Document Collection

Cloud Firestore is a document/collection database like MongoDB or CouchDB. Document/collection means that you create collections and nest documents within those collections. A nested document may itself have a sub-collection of documents.

Unlike a relational database, there are no explicit connections between documents besides this parent/child relationship. If you'd like one document to reference another document, you'll need to save the target document's ID to a field in the referencing document.

Imagine a data structure like this:

- Users
 - userId
 - email
- Transactions
 - userId
 - amount
 
In this case we have a collection of Users and each document has a ```userId``` and an ```email```. Likewise, we have collection of Transactions where each transaction has a ```userId``` and an ```amount```.

The ```userId``` fields can be used to connect records in the two collections, but it's an informal connection defined by your application logic, not by the database itself.

In a relational database you would be able to join Users to Transactions based on the shared userId key. In a document/collection DB that is not possible. Instead, you'd run two queries, one on each collection, and then join the records manually.

### Why not just a relational database?

Relational databases and document/collection databases have quite a bit of overlap. Frankly, for most use cases, either type of database will be just fine... given that you structure your data correctly :)

The advantages of document/collection databases like Firestore is that it is easier to scale, because each document can live on its own. Your database doesn't have to lock down a bunch of relationships to replicate data... it just copies documents across servers. Now, Google Spanner has achieved similar performance for relational data, so it's not like this is a pure win for the document/collection model; however, document/collection does make possible client libraries like the Firestore SDK.

See, relational databases require schemas. Relational database require table updates. Document/collection databases can be flexible, meaning that you can save whatever data you like in a document. The database just does not care about document structure.

So I could make a collection of Foods, and each Food could have entirely different attributes... and the database just wouldn't care.

- Foods
 - Spaghetti
   - noodleType
   - sauceFlavor
 - Sandwich
   - breadType
   - hasPickles
  

### What is a client library?

Traditional application architecture includes a layer of servers that talk to your database. So your client--in this case a browser--will make an HTTP call to your server, which will perform security checks, query the database, and return results to the client.

Serverless application architecture has the client--in this case a browser--communicating directly with the database. The database runs all of its own security checks and serves up data as requested.

The Firestore SDK is a client library that enables your browser to talk directly to the Cloud Firestore database. Firestore handles all of its own security, and the Firestore SDK can read and write whatever your browser asks it to.

This is huge, because you don't have to write any server code. All of that time crafting REST APIs or implementing GraphQL is unnecessary!

### Limitations of Cloud Firestore

Cloud Firestore is not great for highly relational data. 

[Relational databases](https://cloud.google.com/sql/) are optimized for relational data. For example, if you're building an inventory system and your inventory numbers are constantly changing, and each inventory item can belong to a number categories, and you need to be able to quickly run reports on different stock levels for different category groups... you might want to stick to a relational database. 

Cloud Firestore can be sub-optimal for graph data.

Graph databases such as [JanusGraph](http://janusgraph.org/) are optimized for graph data. For instance, if you're building a social app and you find yourself trying to query how many of a user's friends live in nearby cities, but you also need to query how many of those friends' friends also live nearby... you may want to stick to a graph database.

Both of these use cases--relational and graph data--can be modeled in Cloud Firestore; however, the queries may run more slowly and cost more than if you used a more dedicated database. 

### Hybrid solutions

Just because you need a relational database for parts of your data does not mean that you must abandon Firestore!

Most large web applications use a variety of datastores. For instance, a website may use Redis for caching, GraphQL + JanusGraph + Cassandra for social data, SQL for financial data, and Firestore for everything else. Yes, this sort of model adds complexity, and we don't recommend introducing any more complexity to your app than is necessary; however, sometimes complex problems require complex solutions.

Yes, you can drive a large truck to work every day. No, that truck will not be as efficient as driving a small sedan. And yes, a small sedan can transport two cubic yards of sand if you take five trips, but nobody does that. If you need that much sand, you'll rent a truck.

We recommend starting with Firestore until you discover that it won't meet your needs. You may be surprised at how flexible and powerful Firestore can be, and you may never find the need for another database :)

