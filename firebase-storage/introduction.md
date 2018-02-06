# Firebase Storage: Introduction

Every image that you upload online has to get stored somewhere, and cloud storage providers such as Amazon and Google Cloud will store those files as "objects" in "buckets".

The Firebase Realtime Database and Cloud Firestore are great for storing data, but they're not so good with files. Google Cloud Storage, heretofore referred to as Cloud Storage, is built to store and serve these files.

Firebase Storage is a front for Cloud Storage... an extremely useful front.

### The old file-upload pattern

Browsers are great at uploading files, but files are often too big to send over a single HTTP POST, so they're typically streamed to a server. This streaming happens as a series of chunks which the server has to listen for, waiting patiently until the total size of the chunks adds up to the expected file size. The server can then take the file that it has assembled from a bunch of chunks and stream it up to Cloud Storage... again, in a series of chunks.

We've written plenty of file streaming servers, and they're a pain in the neck.

### Firebase Storage saves the day

The old file-upload pattern requires a sophisticated server. And Firebase is all about getting rid of your servers.

Firebase Storage enables you to upload files directly from a browser to Cloud Storage. Firebase Storage handles all of the servers and all of the streaming, leaving you with a simple JavaScript interface.

It seems like a small feature.

Firebase Storage is tiny. 

All it does is upload files. 

But it's hiding a ton of complexity that you'd have to code yourself, so it's a massive win.

### It's just a bucket

Cloud Storage treats each bucket as just that, a bucket.

Cloud Storage does not have a concept of folders.

But you **can** put forward slashes in your filenames, which Firebase Storage will treat as a file path.

For example, we've created a Firebase project named `Quiver Four`; therefore, Firebase Storage automatically creates a Cloud Storage bucket named `quiver-four.appspot.com`.

Let's upload a file to `howtofirebase/uploads/enable-firestore.png`:

```javascript
function uploadFile(file) {
  return firebase
    .storage()
    .ref()
    .child('howtofirebase/uploads/enable-firestore.png')
    .put(file)
    .then(snapshot => {
      // snapshot represents the uploaded file
    });
}
```

And the Firebase Dashboard pretends that the file is in a nested folder!

![storage-browser.png](https://goo.gl/zpYKzh)

Cloud Storage pretends that it's in a folder as well:

![cloud-storage-browser.png](https://goo.gl/j7kVrS)

But then we pulled the file details down using the Cloud Storage SDK and pushed them up to Firestore. Notice that file's name attribute is `howtofirebase/uploads/enable-firestore.png`.

![file-details.png](https://goo.gl/fhm5w5)

### Cloud Storage SDK

Firebase Storage does not have it's own Node.js SDK. It's a browser-only system.

But do not despair! Since these files are merely Cloud Storage objects in a Cloud Storage bucket, we can use the Cloud Storage SDK to interact with them in Node.js.

And we can get Cloud Storage SDK bucket references straight from the Firebase Admin SDK in Node.js!

```javascript
const admin = require("firebase-admin");

const serviceAccount = require("path/to/serviceAccountKey.json");

admin.initializeApp({
    credential: admin.credential.cert(serviceAccount),
    storageBucket: "quiver-foure.appspot.com"
});

const bucket = admin.storage().bucket();
// 'bucket' is a Cloud Storage bucket instance
```

`bucket` is an object defined by the [@google-cloud/storage library](https://cloud.google.com/nodejs/docs/reference/storage/1.5.x/Bucket) for Node.js. The GCP libraries feel different from the Firebase libraries, mostly because the docs look different. But don't be afraid of GCP. It gives you much finer-grained control over its features than Firebase does.



