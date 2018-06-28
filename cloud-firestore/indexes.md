# Indexes

Indexes are required in Cloud Firestore whenever you want to use two where-clauses in a single query.

For example, the following query filters a collection called `uploads` by a `userId` and also orders by a `created` field:

```javascript
async function getUploadsByUser(userId) {
  const query = window.firebase
    .firestore()
    .collection('uploads')
    .where('userID', '==', userId)
    .orderBy('created', 'desc');
  const snapshot = await query.get();
  return snapshot.docs.map(doc => doc.data());
}
```

This query would require indexes on `userId` and `created`.

## Console index management

Queries are super easy to handle via the Console.

The easiest way to do it is to write whatever queries you like and watch your DevTools console for error messages.

You'll see an error message in DevTools whenever you attempt to run a query that requires an index. That error message will come with a link that you can click on to take you to your Console.

Once on the Console, you'll get a prompt to create the required index. Simply confirm and wait for the index to build.

## Firebase Tools index management

The other way to manage indexes is via Firebase Tools.

When you run `firebase init` in your project folder and opt-in to using Firestore, you'll get a file in your project folder named `firestore.indexes.json`.

You'll spec out your indexes in `firestore.indexes.json` and run `firebase deploy` or `firebase deploy --only firestore` to deploy your indexes.

Here's a sample of how your indexes file could look:

```javascript
{
  "indexes": [
    {
      "collectionId": "uploads",
      "fields": [
        { "fieldPath": "userId", "mode": "ASCENDING" },
        { "fieldPath": "created", "mode": "DESCENDING" }
      ]
    }
  ]
}
```

## And that's it!

There's nothing more to Firestore indexes. You just need to be aware of them and adjust them as necessary.

