---
description: Cloud Functions Challenge
---

# Challenge

## Cloud Functions

## Find the repo

We'll be working on a branch of our [firelist-react](https://github.com/how-to-firebase/firelist-react) repo named [challenge-functions](https://github.com/how-to-firebase/firelist-react/tree/challenge-functions).

## Localhost installation

Pull [the repo](https://github.com/how-to-firebase/firelist-react) directly from GitHub...

```text
git clone https://github.com/how-to-firebase/firelist-react.git
cd firelist-react
git checkout challenge-functions
```

## Edit environment files

Update the following files with your own project details:

* `/.firebaserc`
* `/public/environments/environment.dev.js`
* `/public/environments/environment.js`
* `/functions/environments/environment.dev.js`
* `/functions/environments/environment.js`

## Run the tests

Once you're on the branch, make sure to run `yarn` or `npm install` to get your Node.js dependencies.

Then `cd` into your `functions` directory and run `yarn` or `npm install` again.

Make sure that you've installed [nvm for Linux/macOS](https://github.com/creationix/nvm), [nvm for Windows](https://github.com/coreybutler/nvm-windows) or some other tool to switch between Node.js versions.

Use `nvm install 6.14.2` then `nvm use 6.14.2` to switch to Node version 6.14.2.

The Node version is important to make sure that your tests run in an identical environment to that of the Cloud Functions runtime.

Then run `yarn test` or `npm test` to run your tests.

```text
yarn
cd functions
nvm install 6.14.2
nvm use 6.14.2
node --version //should read out 6.14.2
yarn test
```

## Complete the challenge

Search the codebase for `Challenge Functions` to find all of the challenges.

Work through the challenges in order from 01 to 06

