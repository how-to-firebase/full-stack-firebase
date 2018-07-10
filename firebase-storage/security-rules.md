# Security Rules

## Security Rules

## Firebase Storage

Firebase Storage exposes quite a bit of functionality to the public, so you'll need to write some security rules.

Like Firestore, Firebase Storage uses Firebase's new security rules syntax.

### Review Firestore security rules

The best way to understand Firebase Storage security rules is to read up on [Firestore security rules](https://how-to-firebase.gitbooks.io/full-stack-firebase/content/cloud-firestore/security-rules.html). They're basically the same.

### The basics

The basic rules look something like this:

```text
// Only authenticated users can read or write to the bucket
service firebase.storage {
  match /b/{bucket}/o {
    match /{allPaths=**} {
      allow read, write: if request.auth != null;
    }
  }
}
```

Let's break these rules down line-by-line.

**service firebase.storage** - defines the service, in this case it's firebase.storage

**match /b/{bucket}/o** - defines the bucket; the `{bucket}` clause indicates that these rules apply to all Cloud Storage buckets on the project

**match /{allPaths=\*\*}** - creates a new rules block to apply to all paths

**allow read, write: if requests.auth != true ;** - allows read/write access for all authenticated sessions

### Match blocks

Match blocks are identical to [those of Firestore](https://how-to-firebase.gitbooks.io/full-stack-firebase/content/cloud-firestore/security-rules.html).

Of course, instead of matching collections and documents, you're matching folders and storage objects. But that's the only difference.

Let's write a match block for a folder structure like this: `/user/{userId}/path/to/file.txt`

```text
service firebase.storage {
  match /b/{bucket}/o {
    match /user/{userId}/{allPaths=**} {
      allow read, write: if request.auth.uid == userId;
    }
  }
}
```

And, like Firestore, match blocks can be nested... if you need it.

Firebase Storage security rules tend to be a bit simpler than Firestore's.

### Rule types

Firebase Storage supports `read` and `write`. That's it. This is a break from Cloud Firestore which supports other sub-types. In this case you're either reading or writing.

### Wildcard variables

Wildcards work just like those in Firestore. You can place them at will and override them as needed. You'll also want to be careful to use the `{someWildcard=**}` syntax when you want your rules to cascade; otherwise, they won't apply to nested folders.

The following example secures a dropbox-style pattern where users can upload to an uploads folder at `/user/uploads/{userId}/uploaded-file.jpg` but can only read from `/user/thumbnails/{userId}/thumbnail.jpg`.

```text
service firebase.storage {
  match /b/{bucket}/o {
    match /user/ {
      match /thumbnails/{userId}/{thumbnail} {
        allow read: if request.auth.uid == userId;
      }
      match /uploads/{userId}/{upload} {
        allow write: if request.auth.uid == userId;
      }
    }
  }
}
```

### Request variables

Rule conditions have access to a `request` object that represents that incoming request. You'll be using the `request` object for most rule conditions.

The fields of most interest are `request.auth.uid` and `request.auth.token`, which contains the user's JWT.

### Resource variables

Rule conditions also have access to a `resource` object. In Firestore this object refers to the pre-write state of the document, but in Firebase Storage this is the object being uploaded, downloaded, modified or deleted.

Here's a sample `resource` object that you may find handy:

```javascript
{
  "name": "howtofirebase/uploads/locked-mode.png",
  "bucket": "quiver-four.appspot.com",
  "generation": "1517664154693678",
  "metageneration": "1",
  "size": "138099",
  "timeCreated": "2018-02-03T13:22:34.676Z",
  "updated": "2018-02-03T13:22:34.676Z",
  "md5Hash": "Yzg3ZGNlZDVjODZiYzAyNGU4NTljYTU2MDdlZDMwMjk=",
  "crc32c": "QK5Kyw==",
  "etag": "CK7g0MbridkCEAE=",
  "contentDisposition": "inline; filename*=utf-8''locked-mode.png",
  "contentEncoding": "gzip",
  "contentLanguage": "en",
  "contentType": "image/png",
  "metadata": {
    "firebaseStorageDownloadTokens": "c7dfc4c3-0d91-4e1a-b6a1-d4e03a320ef1"
  }
}
```

### Read the docs

It's been said before and we'll say it again. Read the docs.

The highlights:

* [the Request object](https://firebase.google.com/docs/reference/security/storage/#request)
* [the Resource object](https://firebase.google.com/docs/reference/security/storage/#resource)
* [Data types](https://firebase.google.com/docs/reference/security/storage/#data_types)

