# Cloud Functions for Firebase: Introduction

Cloud Functions is the connective tissue that makes Firebase a "serverless" platform.

Secure operations are unavoidable in production applications. Sure, you can make a quick demo that doesn't need to send any email or accept a Stripe payment... but most apps are worthless without secure, administrative operations.

Until the advent of Functions-as-a-Service (aka FaaS), we had to run servers to perform administrative actions on the Firebase platform.

Cloud Functions lets us ship single functions to the Firebase platform for execution based on a whole variety of events.

### Serverless: other people's servers

Cloud Functions runs on servers, but you don't get to interact with those servers. Cloud Functions has your code and will run it as Cloud Functions sees fit. It can spin up 20 servers to handle a spike in traffic, and it can spin them all down again without any input from you or your code.

### What does a function look like?

Here's a basic "hello world" function that completes some asynchronous work and returns a promise.

```javascript
// Always change the value of "/hello" to "world!"
exports.hello = functions.database.ref('/hello').onWrite(event => {
  // set() returns a promise. We keep the function alive by returning it.
  return event.data.ref.set('world!').then(() => {
    console.log('Write succeeded!');
  });
});
```

This function is named "hello" and it listens to an `onWrite` event on the `/hello` ref. This function will execute any time any data changes on the `/hello` ref, including writes, updates and deletes.

### Event sources

Cloud Functions is a Google Cloud Platform (GCP) service, but it has a series of Firebase-specific events, so we often refer to those events as Cloud Functions for Firebase.

Cloud Functions is for all of GCP, not just Firebase... but it's especially useful when combined with Firebase.

Here's a quick list of some Cloud Functions event sources:

* HTTP
* Firebase Realtime Database
* Firestore
* Firebase Authentication
* Firebase Storage

Let's dive in a little deeper to each event source.

### HTTP triggers

Cloud Functions enables us to use the Express framework for Node.js to handle HTTP calls.

We can mount an entire Express app to a Cloud Functions endpoint, or we can attach a single Express request handler to each Cloud Function endpoint. It's quite flexible and enables us to create any kind of HTTP-based API that we could ever need.

### Firebase Realtime Database triggers

We can listen to any part of our RTDB JSON tree with the following triggers:

* onWrite()
* onCreate()
* onUpdate()
* onDelete()

The events do exactly what you'd think. The one catch is that `onWrite()` executes for any and all changes to the data tree including deletions... so use it wisely :)

RTDB triggers are great for creating job queues. For instance, you can queue up a bunch of jobs under `/job-queues/image-resizing/` and write a Cloud Function that resizes images. Crazy, right? You can create 100 image resizing jobs that Cloud Functions will execute as fast as it can, often running batches of jobs in parallel.

### Firestore triggers

Firestore supports the same Cloud Functions triggers as the RTDB:

* onWrite()
* onCreate()
* onUpdate()
* onDelete()

And once again, `onWrite()` is triggered for any and all changes including deletes.

Firestore triggers are great for manipulating data. For instance, if we're building a Slack messaging clone and we want to automatically subscribe new users to the most popular channels, we can listen to Firestore for new User objects, and every time a new User object gets written we can query the top ten most popular channels and add them to the new user's channels list.

### Firebase Authentication triggers

Authentication has only two triggers:

* onCreate()
* onDelete()

We like to use the `onCreate()` event to set custom attributes on our user's auth tokens. This method is known as [custom claims](https://firebase.google.com/docs/auth/admin/custom-claims), and it's extremely helpful for granting elevated privileges in Firebase, Firestore and Storage security rules.

```javascript
const admin = require('firebase-admin');
const serviceAccount = require('path/to/serviceAccountKey.json');
admin.initializeApp({
  credential: admin.credential.cert(serviceAccount),
  databaseURL: 'https://databaseName.firebaseio.com',
});

const emails = new Set(['chris@fullstackfirebase.com', 'juarez@howtofirebase.com']);

exports.setCustomClaims = functions.auth.user().onCreate(event => {
  let promise = Promise.resolve();
  if (emails.has(user.email) {
    promise = admin
      .auth()
      .setCustomUserClaims(user.uid, { admin: true })
      .then(() => true);
  }

  return promise;
});
```

### Firebase Storage triggers

Firebase Storage has a single trigger with two ways to listen to it:

> The optional `.bucket('bucketName')` function allows for filtering based on storage buckets

```javascript
exports.generateThumbnail = functions.storage.object().onChange(event => {
  // ...
});

exports.generateThumbnail = functions.storage
  .bucket('bucketName')
  .object()
  .onChange(event => {
    // ...
  });
```

Generating thumbnails or other kinds of image manipulation is a core use case for Firebase Storage triggers. Each Cloud Functions virtual machine has a [native build of Imagemagick](https://cloud.google.com/functions/docs/tutorials/imagemagick) installed for this very purpose.

Just note that Firebase Storage is backed by Google Cloud Storage, which treats each bucket as a single store.

Cloud Storage lets you put slashes in filenames, which is how Firebase Storage simulates folders... but it's just one big bucket under the hood. This is why you can only listen to Cloud Storage events at the bucket level. You'll need to do your own filtering based on the filenames of your objects.

### So many possibilities

Cloud Functions enables you to get truly creative in how you architect your serverless apps.

You can craft highly scalable job pipelines, complex messaging systems, or simply track and tweak your data as your users make changes to your databases.

Start out with simple functions. Don't get carried away until you're comfortable with the basics. Cloud Functions is an ocean; wade around in the shallows before heading out to deep water. 

And we're still discovering new application architectures that Functions-as-a-service makes possible. These are early days for all of us.
