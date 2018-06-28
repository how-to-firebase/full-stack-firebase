# Introduction

The Firebase Realtime Database \(aka "the RTDB"\) is the original database that shot Firebase onto the scene in 2012.

The RTDB was one of the first "serverless" datastores on the market, competing directly with Parse and eventually with RethinkDB's Horizon project.

The original innovation was to enable web browsers to connect directly to the database with a realtime web socket connection. Browsers have made HTTP calls out to API services for years, but now, instead of making manual API calls, your browser could subscribe to the database directly and receive live updates.

## JSON Datastore

The RTDB is neither a relational database nor a document/collection datastore like Cloud Firestore.

The RTDB is a single JSON object with up to 32 levels of depth, and you can subscribe to any node of that JSON object, whether it already exists or has yet to be made.

This sort of JSON datastore makes the RTDB entirely unstructured. You can define whatever data structures you prefer in your browser and then save those data structures directly to the RTDB without modification and without telling the RTDB what the data will look like.

Unstructured data can make development lightning fast... if you know what you're doing.

If you're naive in how you structure your data, then you're in for very unpleasant surprises.

## Strengths

The Realtime Database is ideal for realtime interactions. You could write your own web sockets server, but why? Web sockets are wicked hard to get working smoothly, and Firebase has already done the work for you.

The RTDB is great for small, fast transactions. Think of the ideal RTDB use case as streaming data. You stream data to the RTDB and it streams that data out to all connected clients in a single, realtime stream.

In fact, the entire Realtime Database SDK is evented. It treats data as event streams, not as fixed queries.

## Weaknesses

RTDB devotees such as ourselves spent the years of 2012 through 2017 learning to model all sorts of creative data structures in pure JSON. It hasn't always been easy.

We learned to host entire production applications on the RTDB long before Firestore came along to give us a more structured data model. We can model almost anything in JSON...but we don't have to do that anymore, so we'll completely avoid the RTDB's weaknesses by using Firestore for all of those use cases.

## Why is the Realtime Database so limited?

The RTDB was designed for lightning fast updates. That primary design constraint prevented the Firebase team from developing sophisticated query capabilities.

You can execute a single order-by filter and a single limit filter on each RTDB data stream. So you can order a list of movies by release date and request a live stream of the 10 most recent movies, but you can't also filter that stream to include only action movies. You already used your one order-by filter on the release date. Tough luck!

Of course, you **can** create a new collection of movieTypes, each of which has movies ordered by release date... so your movieTypes could include action, drama and comedy, in which case you could then query the action movies list and order by release date.

That achieves the same goal, but it adds a bunch of complexity to how you write your data, because now each movie has to be duplicated from the primary movies list to the appropriate movieTypes list. And what if you also want to filter by MPAA ratings? Get prepared to duplicate your data once again.

## How do we use the Realtime Database?

We use the RTDB for streaming short-lived, constantly-changing data.

We use it for tracking logged-in users.

We use it for live progress notifications for long-running Cloud Functions.

We use it for Cloud Functions job queues.

And that's about it.

There are plenty of other applications where the RTDB still outperforms Cloud Firestore, and we still have plenty of apps in production that use the RTDB exclusively; however, our new projects store &gt;90% of their data in Cloud Firestore.

## Realtime Database Cost

The RTDB is billed based on the gigabytes of data stored and the gigabytes transferred. This makes it ideal for smaller datasets with massive transaction counts. The RTDB doesn't care how many reads and writes you make. It cares about the volume of data travelling over the network.

Don't store large volumes of data in the RTDB. That's not what it's built for, and you'll pay $5/GB every month. That's nothing if you're streaming bytes of short-lived data. Use Cloud Firestore for everything else.

