# Security Rules

The Realtime Database is public facing, so it needs a robust security system.

Firestore and Firebase Storage both use Firebase's new security rules syntax, which we've covered elsewhere. Those learnings won't transfer to the RTDB, because the RTDB's security rules were designed back in 2011 and are specific to its JSON data model.

## Follow the JSON

The RTDB stores data in JSON. It's security rules are also written in JSON and follow the pattern of the data that you plan on storing.

The starting JSON rules object is pretty simple:

```javascript
{
  "rules": {
    ".read": "auth != null",
    ".write": "auth != null"
  }
}
```

Notice that there's a root attribute named `rules` and there are two kinds of permissions, `.read` and `.write`. There's also an `auth` object available in the rule conditions which we can test to make sure that it's not null. If it's not null, then the request must be coming from an authenticated user!

Now let's secure the `users` node on our imaginary JSON tree.

**Data**

```javascript
{
  "users": {
    "userOne": {...},
    "userTwo": {...}
  }
}
```

**Rules**

```javascript
{
  "rules": {
    "users": {
      ".read": "auth != null",
      ".write": false
    }
  }
}
```

Notice how we created a new node under `rules` and we called that node `users`? Yeah, we're nesting our rules to match our data. So `rules.users` matches our `users` node in the JSON.

## Rule types

The RTDB has only three rule types:

* `.read`
* `.write`
* `.validate`

`.read`, `.write` and `.validate` must all be set to `true`, `false` or a string "condition" that the security rules engine will need to evaluate.

`.read` and `.write` are simple enough. They grant read or write access.

`.validate` will prevent a write if it's condition statement evaluates to false.

The RTDB has one more type, although we wouldn't call it a rule so much as a directive:

* `.indexOn`

The `.indexOn` field is necessary to speed up RTDB queries, and it's either the string `".value"` or an array of strings representing the attributes to index.

`.indexOn` could be used like this:

```javascript
{
  "rules": {
    "scores": {
      ".indexOn": ".value"
    },
    "teams": {
      ".indexOn": ["ranking", "dateCreated", "mascotColor"]
    }
  }
}
```

## Wildcards

Wildcards are an important concept in RTDB security rules. Here's an example of a `$userId` wildcard:

```javascript
{
  "rules": {
    "users": {
      "$userId": {
        // grants write access to the owner of this user account
        // whose uid must exactly match the key ($user_id)
        ".write": "$userId === auth.uid"
      }
    }
  }
}
```

The dollar sign in `$userId` indicates that it's a wildcard. You an name wildcards whatever you like. You could have named it `$broccoli`... but then you'd have to refer to the wildcard as `broccoli` in your condition statements :\)

Wildcards apply to all otherwise-unspecified attributes. So assume the following data:

```javascript
{
  "users": {
    "userOne": {...},
    "userTwo": {...},
    "userThree": {...},
  }
}
```

We have three users. Now let's imagine all user data is public except that of our admin, `userThree`. Also let's assume that we want `userThree` to be able to write to everyone's records as well as read and write to her own.

```javascript
{
  "rules": {
    "users": {
      "$userId": {
        ".read": true,
        ".write": "auth.uid === 'userThree'"
      },
      "userThree": {
        ".read": "auth.uid === 'userThree'",
        ".write": "auth.uid === 'userThree'"
      }
    }
  }
}
```

We used the `$userId` wildcard to set rules for all users. Then, by specifying `rules.users.userThree`, we overrode the wildcard for `userThree`.

## Cascading rules

Be very aware when designing your data models that RTDB security rules cascade.

Let's modify the earlier example a bit and try to block all users from reading `users.$userId.superSecretUserAttributes`.

```javascript
{
  "rules": {
    "users": {
      "$userId": {
        ".read": true,
        ".write": "auth.uid === 'userThree'",
        "superSecretUserAttributes": {
          ".read": "auth.uid === 'userThree'"
        }
      }
    }
  }
}
```

Remember how rules cascade? Well, our attempt at securing `superSecretUserAttributes` just failed, because we granted `.read` access higher up the chain. In this example, all users can still read `superSecretUserAttributes` and the nested `.read` rule gets ignored.

## Design your data with cascading rules in mind

Cascading security rules mean that you should design your data structure such that you never need to nest security rules. Here's our favorite way to write rules:

```javascript
{
  "rules": {
    "users": {
      "$uid": {
        ".read": "auth.uid === $uid || auth.token.admin === true",
        ".write": "auth.uid === $uid || auth.token.admin === true"
      }
    },
    "userOwned": {
      "$objectType": {
        "$uid": {
          ".read": "auth.uid === $uid || auth.token.admin === true",
          ".write": "auth.uid === $uid || auth.token.admin === true"
        }
      }
    },
    "userReadable": {
      "$objectType": {
        "$uid": {
          ".read": "auth.uid === $uid || auth.token.admin === true",
          ".write": "auth.token.admin === true"
        }
      }
    },
    "userWriteable": {
      "$objectType": {
        "$uid": {
          ".read": "auth.token.admin === true",
          ".write": "auth.uid === $uid || auth.token.admin === true"
        }
      }
    },
    "adminOwned": {
      "$objectType": {
        "$uid": {
          ".read": "auth.token.admin === true",
          ".write": "auth.token.admin === true"
        }
      }
    },
    "public": {
      "$objectType": {
        "$uid": {
          ".read": true,
          ".write": "auth.token.admin === true"
        }
      }
    }
  }
}
```

And that's it! We have six base nodes in our data structure:

```javascript
{
  "users": {...},
  "userOwned": {...},
  "userReadable": {...},
  "userWriteable": {...},
  "adminOwned": {...},
  "public": {...}
}
```

And notice how each base node has a wildcard `$objectType` nested directly underneath it? That's so we can save lots of different types of objects, all of which will inherit their rules from their parent nodes.

Instead of fighting the cascading rules, we're using them to our advantage and dramatically reducing the number of rules that we write.

We're leveraging custom claims to permit users with the `admin` claim to read and write everything using `auth.token.admin === true`. [Custom claims](https://firebase.google.com/docs/auth/admin/custom-claims) are awesome, because they're available across all three types of security rules—Firestore, RTDB and Storage—as well as your browser's `currentUser` JWT.

## Validation

Frankly, we do most of our validation in our client-side applications; however, the RTDB's `.validate` security rule will let you do validation right at your data layer.

If you find yourself in need of validation, we recommend reading the [reference docs](https://firebase.google.com/docs/reference/security/database/#validate) carefully. We also recommend not getting too carried away with validation in the security rules. These rules are better seen as a flexible way to add security to your app. If you have some sensitive data or operations and you need more assurances than the `.read` and `.write` rules can give you, it's time for some `.validate` rules.

We're sure that we _could_ write a sophisticated validation layer using the security rules, but we don't. We use client-side validation &gt;99% of the time, because the vast majority of our writes aren't likely to be abused by an attacker. Any competent hacker will connect directly to your database and start testing your endpoints, so client-side validation won't stop hacking; however, it's easy enough to hide anything worth hacking deep within a cloud function or an admin-only data node, so try that first before relying on validation rules.

