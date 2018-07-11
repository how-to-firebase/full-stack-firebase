---
description: Firebase Storage Challenge
---

# Challenge

## Firebase Storage

## Find the repo

We'll be working on a branch of our [firelist-react](https://github.com/how-to-firebase/firelist-react) repo named [challenge-storage](https://github.com/how-to-firebase/firelist-react/tree/challenge-storage).

## Localhost installation

Pull [the repo](https://github.com/how-to-firebase/firelist-react) directly from GitHub...

```text
git clone https://github.com/how-to-firebase/firelist-react.git
cd firelist-react
git checkout challenge-storage
```

## Edit environment files

Update the following files with your own project details:

* `/.firebaserc`
* `/public/environments/environment.dev.js`
* `/public/environments/environment.js`
* `/functions/environments/environment.dev.js`
* `/functions/environments/environment.js`

## Deploy Cloud Functions

If you had some trouble deploying Cloud Functions earlier, make sure to deploy the correct functions now.

Run `yarn deploy:functions` to deploy valid versions of each function up to your Firebase project.

## Start the app

Once you're on the branch, make sure to run `yarn` or `npm install` to get your Node.js dependencies.

Then run `yarn start` or `npm run start` to spin up the development server.

```text
yarn
yarn start
```

## Complete the challenge

Search the codebase for `Challenge Storage` to find all of the challenges.

Read the comments and complete the steps in those files.

- `src/storage/delete-image.js`
- `src/storage/get-upload-observable.js`

