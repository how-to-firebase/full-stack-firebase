# Challenge

## Firebase Hosting

## Find the repo

We'll be working on the [master branch](https://github.com/how-to-firebase/firelist-react) of our [firelist-react](https://github.com/how-to-firebase/firelist-react) repo.

## Localhost installation

Pull [the repo](https://github.com/how-to-firebase/firelist-react) directly from GitHub...

```text
git clone https://github.com/how-to-firebase/firelist-react.git
cd firelist-react
git checkout master
```

## Edit environment files

Update the following files with your own project details:

* `/.firebaserc`
* `/functions/environments/environment.dev.js`
* `/functions/environments/environment.js`
* `/public/environments/environment.dev.js`
* `/public/environments/environment.js`

## Build the app

Once you're on the branch, make sure to run `yarn build` or `npm install` to get your Node.js dependencies.

Build the app with `yarn build` or `yarn build:windows`.

## Inspect firebase.json

Read through `/firebase.json` and try to understand the settings.

## Deploy the app

Just run `yarn deploy`!

Check out `/package.json` and the `scripts` attribute to see what scripts are available and what they do.




