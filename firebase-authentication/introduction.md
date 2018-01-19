# Introduction: Why Firebase Authentication?

You need Firebase Authentication for two reasons:

1. Firebase Security Rules rely on Firebase Authentication
2. Firebase Authentication is ridiculously easy to use

## Firebase Auth + Security Rules

Firebase needs a security system. In a traditional database you provide your own security using your
API server. Since Firebase **Is** the API server, it needs a programmable way to control read and
write access to your data. When users use your client apps to authenticate with Firebase
Authentication they receive a [JSON Web Tokens](https://jwt.io/) that will identify them to
Firebase's Security Rules system. We'll cover this more later.

## Ease Of Use

Have you ever implemented your own auth system? Yes? Then you know how challenging it can be. If
not... then take my word for it and use an off-the-shelf system. Firebase Authentication provides
the following auth methods:

* Email/password
* Phone
* OAuth 2
  * Google
  * Facebook
  * Twitter
  * Github
* Anonymous
* Custom auth

![Imgur](https://i.imgur.com/5K9DW4z.png)

All of these methods use [JSON Web Tokens](https://jwt.io/), also known as JWTs.

> JWT is pronounced the same as the English word "jot".

We'll cover JWTs later. Don't worry. They're relatively simple.

### Email/Password

Email/password auth is exactly what it sounds like. You register an email address and a password
with Firebase and it keeps track of your user account.

### Phone

Google [acquired Twitter Digits](https://firebase.googleblog.com/2017/01/FabricJoinsGoogle17.html)
in early 2017 and rolled their phone authentication into Firebase Authentication. This means that
with minimum fuss you can implement a full SMS-based phone auth flow. This is particularly great for
mobile web apps. Phone authentication is the preferred auth method for many users, especially those
outside of the United States.

### OAuth 2 (Google, Facebook, Twitter, Github)

OAuth is the fastest auth method, because it relies on accounts that your users already have. Most
everyone has either Google, Facebook or Twitter, and developers love Github. OAuth 2 is also the
easiest auth flow to implement.

And all of the OAuth providers support
[multi-factor authentication](https://en.wikipedia.org/wiki/Multi-factor_authentication), which we
should all be using.

## Anonymous Auth

You may want to interact with a client that hasn't authenticated. If you're developing a shopping
cart feature for your app, you may want users to add items to their carts before they've created an
account. This is where anonymous auth comes in.

Without some sort of authentication, there's no way for your server to know who it's talking to.
Firebase's data is either entirely open or requires authentication. Any private transaction between
the Firebase database and a client app requires authentication or it will be public and could be
intercepted by a malicious third party.

There are situations in which you might make your data publicly accessible; however, user
transactions with your database are unlikely to fit this model... and that's where anonymous auth
comes in.

We won't cover anonymous auth any further except to show how dead simple it is to execute:

```javascript
firebase
  .auth()
  .signInAnonymously()
  .then(successHandler, errorHandler);
```

### Custom Tokens

Would you like to use a different authentication method with Firebase? That's not a problem. We
won't cover custom tokens here except to say that you can use your auth server to mint Firebase auth
tokens that you can give to your client applications. Those tokens will allow your client apps to
authenticate with Firebase using whatever JWT you chose to create on your server.

## Link Multiple Auth Providers

This is another topic we won't cover here. Just be aware that you can easily
[link multiple auth providers to a single account](https://firebase.google.com/docs/auth/web/account-linking).

For example, your user might register an email/password account and you could prompt her to link her
account to Google and Facebook. Your client app can make a couple of easy calls to Firebase
Authentication to initiate the linking procedure, and now your user can sign in with Google,
Facebook, Twitter or Github.

Alternatively, if your user signed up with an OAuth account, you can prompt her to register a linked
email/password combination.

