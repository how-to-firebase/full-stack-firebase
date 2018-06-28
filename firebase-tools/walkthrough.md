# Walkthrough

You'll get started with Firebase Tools by installing it. Open up your terminal and make sure you have Node.js installed:

```bash
$ node --version 
$ #some version number
```

If you see `bash: command not found: node`, then you'll need to [install Node.js](https://nodejs.org/en/download/).

Now it's time to install Firebase Tools.

```bash
$ npm -g install firebase-tools
```

To confirm successful installation:

```bash
$ firebase --version 
$ #some version number
```

To get help:

```bash
$ firebase --help
```

## Initialize your app

We mostly use Firebase Tools to initialize our apps. Just make a new folder, `cd` into it, and call `firebase init`.

```bash
mkdir my-new-project
cd my-new-project
firebase init
```

Select all of the Firebase features. Because why not!

![firebase-init.png](https://goo.gl/FbF1Zi)

Now follow the prompts, sticking to the defaults with one exception:

```bash
? Configure as a single-page app (rewrite all urls to /index.html)? (y/N) y
```

It's easy to remove the single-page app rewrites later if you don't need them. Your should `firebase.json` file should look something like this:

```javascript
{
  "database": {
    "rules": "database.rules.json"
  },
  "firestore": {
    "rules": "firestore.rules",
    "indexes": "firestore.indexes.json"
  },
  "hosting": {
    "public": "public",
    "ignore": ["firebase.json", "**/.*", "**/node_modules/**"],
    "rewrites": [
      {
        "source": "**",
        "destination": "/index.html"
      }
    ]
  },
  "storage": {
    "rules": "storage.rules"
  }
}
```

## Deploy

Run `firebase deploy` to deploy your new project.

You may get an error like...

```text
Error: HTTP Error: 400, Project 'how-to-firebase-tutorials' is not a Firestore enabled project.
```

... in which case, you'll need to use your Console to activate the Firestore Beta. Make sure to start in locked mode.

![enable-firestore.png](https://goo.gl/veDwFU)

![locked-mode.png](https://goo.gl/AQDVgp)

Now try `firebase deploy` again if necessary.

Notice the final line of output that looks like this:

```text
Hosting URL: https://how-to-firebase-tutorials.firebaseapp.com
```

Follow that url to see the results!

## Deploy pipeline

The Node.js community has moved toward putting all deploy commands in a `package.json` file, so let's initialize our project for Node.js as well and set up our deploy pipeline.

```bash
$ npm init #go ahead and accept the defaults
```

We've already installed Firebase Tools globally with `npm -g install firebase tools`. Now that we have a local `package.json` file, let's install it locally as well.

```bash
npm install --save-dev firebase-tools
```

Now that we have a local version of `firebase-tools` our `package.json` scripts will run using the local version instead of the global version. This is super helpful if you copy your code to a machine that doesn't have Firebase Tools installed globally.

Now let's add some scripts:

```javascript
{
  "name": "starter-project",
  "version": "1.0.0",
  "main": "index.js",
  "license": "MIT",
  "devDependencies": {
    "firebase-tools": "^3.17.4"
  },
  "scripts": {
    "deploy": "firebase deploy",
    "deploy:hosting": "firebase deploy --only hosting",
    "deploy:database": "firebase deploy --only database",
    "deploy:firestore": "firebase deploy --only firestore",
    "deploy:storage": "firebase deploy --only storage",
    "deploy:functions": "firebase deploy --only functions"
  }
}
```

We've added a `scripts` attribute and created scripts for each kind of deploy that we could use. We won't need to call `firebase deploy` directly anymore. Instead we'll call `npm run deploy` or `npm run deploy:hosting`.

This is a great habit to promote. It both documents and standardizes your deploy pipeline so that you can come back to this project in two years and know exactly how to deploy it.

Try running `npm run deploy:hosting` to test it out!

## Firebase deploy

Firebase Tools deploys five different modules, each of which can be deployed individually with the `--only` flag:

* hosting
* database
* firestore
* storage
* functions

### Hosting

Hosting deploys your public folder to Firebase Hosting. It also deploys any hosting settings you have in `firebase.json`.

### Database

The database module deploys your security rules to the Realtime Database as defined in `database.rules.json`.

### Firestore

The firestore module deploys two files, `firestore.rules` and `firestore.indexes.json`. These files contain security rules and index specifications.

### Storage

The storage module deploys the security rules defined in `storage.rules`, which you'll use to secure Firebase Storage.

### Functions

The functions module is the trickiest of the bunch. It runs a bunch of checks on your `/functions` folder to make sure that all of the right NPM packages are installed and that `/functions/index.js` exports valid Cloud Functions.

Firebase Tools will deploy your functions to Cloud Functions once its validation checks pass. But beware! Firebase Tools can't validate your code very deeply. It just makes sure that you're importing modules correctly and attaching functions to valid endpoints.

We _**HIGHLY**_ recommend--in all caps no less--developing Cloud Functions in a local testing environment. Don't get sucked into the tempting feedback loop of deploying a function, testing it on the Cloud Functions servers, making edits and deploying again... This is your ticket to Cloud Functions hell 😈

We'll cover Cloud Functions test-driven development elsewhere. You have been warned! 🎉🎉🎉

