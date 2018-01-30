# Cloud Firestore: Introduction

Cloud Firestore is the successor to both Cloud Datastore and the original Firebase Realtime Database.

Cloud Datastore already powers massive web applications, and the Realtime Database proved the necessity of direct browser-to-database interactions. Cloud Firestore combines the scalability and reliability of Cloud Datastore with the awesome client libraries of the Realtime Database to create a massively scalable, easily-to-develop cloud database platform.

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

The Firestore SDK is a client library that enables your browser to talk directly to the Cloud Firestore database. This is huge, because you don't have to write any server code.