# Cloud Firestore: Security Rules

A public-facing database wouldn't be complete without a security system.

Firestore and Firebase Storage both use Firebase's new security rules syntax, which the original Firebase Realtime Database uses the original JSON security rules syntax. Both systems are easy enough to work with.

The gist of security rules is that you'll be granting read and/or write access to individual nodes of your database.

### Basic Rules

Our Firestore security rules for Fogo, our image-sharing app, are as follows:

```
service cloud.firestore {
  match /databases/{database}/documents {
    match /uploads/{document=**} {
      allow write: if request.auth.token.admin == true ;
      allow read;
    }

    match /users/{document=**} {
      allow read, write: if request.auth.token.admin == true ;
    }
  }
}
```

Let's break these rules down line-by-line.

**service cloud.firestore** - defines the service, in this case it's cloud.firestore

**match /databases/{database}/documents** - defines the database; the `{database}` clause indicates that these rules apply to all Firestore databases on the project

**match /uploads/{document=\*\*}** - creates a new rules block to apply to the _uploads_ collection and all documents contained therein

**allow write: if requests.auth.token.admin == true ;** - allows write access for authenticated sessions with an `admin` attribute equal to `true` on the auth token, which is also known as the user's JWT

**allow read;** - allows public read access

**match /users/{document=\*\*}** - creates a new rules block for the _users_ collection and all documents contained therein

**allow read, write: if request.auth.token.admin == true ;** - allows both read and write access for authenticated sessions with an `admin` attribute equal to `true` on the auth token, which is also known as the user's JWT

### Match blocks

Here's the pattern for match blocks 👇

```
match /my-collection/my-document {

}
```

The document-name fields can be set to wildcard values as well, which looks like this 👇

```
match /my-collection/{allDocuments} {

}
```

If you want a rule to apply to all documents _AND_ all sub-collection documents, you need a slightly different syntax:

```
match /my-collection/{allDocuments=**} {

}
```

Notice the `=**` at the end of the wildcard? That's the "recursive wildcard" syntax. If you don't tell your wildcard to be recursive, your match block will not apply to sub-collection documents. Imagine a data structure like `/users/{userDocs}/preferences/{preferenceDocs}`, where you have a collection of Users, and each user has a collection of Preferences. We can think of three types of match blocks for this data structure:

```
match /users/{user} {
  // applies to the user docs, NOT the nested preference docs
}

match /users/{user=**} {
  // applies to just the user docs AND the nested preference docs
}

match /users/{user}/preferences/{preference} {
  // applies ONLY to the preference docs
}
```

You could write the same rules like this:

```
match /users/{user} {
  // applies to the user docs, NOT the nested preference docs

  match /preferences/{preference} {
    // applies only to the preference docs
  }
}
```

See what we did there with the nested rule blocks? Yeah, you can nest match blocks. It's purely optional, but it might be easier to read in some cases.

### Rule types

The basic rule types are `read` and `write`. But each of these rule types can be broken down into sub-types.

* read
  * get
  * list
* write
  * create
  * updated
  * delete

#### Read rules

The basic `allow read` rule grants both `get` and `list` access to the documents in a collection.

The `allow get` rule allows a user to read a single document, but not list all documents.

And `allow list` allows a user to read an entire collection or query the collection.

There's an funny bit of detail here. Imagine allowing a user to `list` a collection but not `get` a document within it. This would be a strange rule, because that user would still be able to read each individual document... but he or she would have to pull the entire collection rather than one specific document. There might be a use case for this pattern, but we can't think of one.

The `get` and `list` rules would be useful if, for example, you want an admin user to both get and list all of the documents in a collection, but you want regular users to only get certain documents that are specific to them. In this model, you'd have to provide the ID of the document to the user.

Here's how those queries could look:

```javascript
firebase
  .firestore()
  .collection('users')
  .get()
  .then(snapshot => {
    // allowed for an admin user
  })
  .catch(error => {
    // a non-admin user is denied list permission
  });

firebase
  .firestore()
  .collection('users')
  .doc('my-user-id')
  .get()
  .then(snapshot => {
    // a non-admin user can get just one doc
  });
```

#### Write rules

The `allow write` rule grants `create`, `update` and `delete` privileges.

The `allow create`, `allow update` and `allow delete` rules are self-explanatory 😊

Imagine a project-management app with three levels of user-security: admins, managers and employees. In this case, you may want to allow employees to update existing projects, allow managers to create and update projects and allow the admins full create, update and delete permissions.

### Conditions

Perhaps the trickiest part of security rules is writing the individual rule conditions.

To start off, you can always declare a rule without permissions:

```
match /dropboxCollection {
  read false;
  write true;
}

match /mailboxCollection {
  read true;
  write false;
}
```

But most apps need to write conditions, so security rules have a JavaScript-inspired DSL (domain-specific language) for defining those conditions.

First off, we're going to need variables.

### Wildcard variables

First off, any wildcards that you declared in your match rules are available as variables.

The following example shows how you could enable users to read their own user documents and write only one preference document: `receiveMarketingEmail`;

```
match /users/{userId} {

  allow read: if request.auth.uid == userId;

  match /preferences/{preference} {  
    allow read: if request.auth.uid == userId;
    allow write: if request.auth.uid == userId && preference == 'receiveMarketingEmail';
  }
}
```

### Request variables

Rule conditions have access to a `request` object that represents that incoming request. You'll be using the `request` object for most rule conditions. Here's a sketch of what that object looks like as JSON:

```json
{
  "auth": {
    "uid": "my-unique-user-id-aka-uid",
    "token": {
      "some-custom-claim": true,
      "email": "user@email.com",
      "email_verified": false,
      "phone_number": null,
      "name": "My displayName",
      "sub": "my-unique-user-id-or-uid",
      "firebase": {
        "identities": {
          "google.com": ["first-google-uid", "second-google-uid"],
          "facebook.com": ["first-facebook-uid", "second-facebook-uid"]
        },
        "sign_in_provider": "facebook.com"
      }
    }
  },
  "path": Path,
  "query": {
    "limit": 10,
    "offset": "some-cursor-value",
    "groupBy": {
      "widgetType": true,
      "widgetName": false
    },
    "orderBy": {
      "widgetCreated": "ASC",
      "widgetName": "DESC"
    },
    "resource": Resource
  },
  "time": Timestamp,
  "writeFields": List
}
```

The `request.auth` attribute is mostly primitive values, i.e., strings, numbers, nulls and booleans. Note that you can access custom claims directly off of `request.auth.token`.

Now we get into some custom objects. You'll want to follow links and read the docs.

`request.path` is a [Path object](https://firebase.google.com/docs/firestore/reference/security/#path_2).

`request.query.resource` is a [Resource objects](https://firebase.google.com/docs/firestore/reference/security/#firestore-resource) representing the changes being made by a write.

`request.time` is a [Timestamp object](https://firebase.google.com/docs/firestore/reference/security/#timestamp).

And finally, `request.writeFields` contains a [List object](https://firebase.google.com/docs/firestore/reference/security/#list) of fields being written.

### Resource object

In addition to the `request` object, there's also a `resource` object available. That means that `resource.data`  can be compared to `request.resource.data` to identify requested changes.

### Stick with request.auth

The `request.auth` object is by far the most commonly used part of the `request` object, especially when [custom claims](https://firebase.google.com/docs/auth/admin/custom-claims) have been set on `request.auth`. 

A common custom claim to set is an `admin` flag which can be used like so:

```
match /superSecretAdminStuff/{docs=**} {
  allow read, write: request.auth.token.admin == true;
}
```

### Read the docs

Firebase has excellent docs, and we don't want to compete with such great writing.

The highlights:

- [the Resource object](https://firebase.google.com/docs/firestore/reference/security/#firestore-resource)
- [Functions: exists() and get()](https://firebase.google.com/docs/firestore/reference/security/#functions)
- [Developer-defined Functions](https://firebase.google.com/docs/firestore/reference/security/#developer_defined)
- [Data types](https://firebase.google.com/docs/firestore/reference/security/#data_types)

### Firebase Storage security rules are nearly identical

Firestore's security system is shared by [Firebase Storage](https://firebase.google.com/docs/reference/security/storage/). The objects and data types are identical, but Storage is dealing with binary objects, so it's Resource properties are bit different.
